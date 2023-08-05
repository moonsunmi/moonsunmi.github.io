---
layout: post
title: "점프 투 장고 공부 11"
date: 2022-08-28 22:45:13 +0900
tags: jumptodjango
---

## 앵커

답글을 작성하거나 수정한 후에 항상 페이지 상단으로 스크롤이 이동되기 때문에 본인이 작성한 답변을 확인하려면 다시 스크롤을 내려서 확인해야 한다는 점을 해결한다.

answer_views.py
{% raw %}

```python

@login_required(login_url='common:login')
def answer_create(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    if request.method == "POST":
        form = AnswerForm(request.POST)
        if form.is_valid():
            answer = form.save(commit=False)
            answer.author = request.user  # author 속성에 로그인 계정 저장
            answer.create_date = timezone.now()
            answer.question = question
            answer.save()
            return redirect('{}#answer_{}'.format(
                resolve_url('pybo:detail', question_id=question.id), answer.id))
    else:
        return HttpResponseNotAllowed('only POST is allowed')
    context = {'question': question, 'form': form}
    return render(request, 'pybo/question_detail.html', context)


@login_required(login_url='common:login')
def answer_modify(request, answer_id):
    answer = get_object_or_404(Answer, pk=answer_id)

    if request.user != answer.author:
        messages.error(request, '수정 권한이 없습니다.')
        return redirect('{}#answer_{}'.format(
            resolve_url('pybo:detail', question_id=answer.question.id), answer.id))

    if request.method == 'POST':
        form = AnswerForm(request.POST, instance=answer)
        if form.is_valid():
            answer = form.save(commit=False)
            answer.modify_date = timezone.now()
            answer.save()

        return redirect('{}#answer_{}'.format(
            resolve_url('pybo:detail', question_id=answer.question.id), answer.id))
    else:
        form = AnswerForm(instance=answer)
    context = {'answer': answer, 'form': form}
    return render(request, 'pybo/answer_form.html', context)


@login_required(login_url='common/login')
def answer_vote(request, answer_id):
    answer = get_object_or_404(Answer, pk=answer_id)

    if request.user == answer.author:
        messages.error(request, '자신의 댓글을 추천할 수 없습니다.')
    else:
        answer.voter.add(request.user)

    return redirect('{}#answer_{}'.format(
        resolve_url('pybo:detail', question_id=answer.question.id), answer_id))

```

{% endraw %}

`resolve_url`은 실제 호출되는 URL 문자열을 리턴하는 장고의 함수이다.

## 마크다운

pybo_filter.py

{% raw %}

```python
import markdown
from django import template
from django.utils.safestring import mark_safe

register = template.Library()


@register.filter
def sub(value, arg):
    return value - arg



@register.filter
def mark(value):
    extensions = ["nl2br", "fenced_code"]
    return mark_safe(markdown.markdown(value, extensions=extensions))
```

{% endraw %}

question_detail.html
{% raw %}

```html
<div class="card-text">{{ answer.content|mark }}</div>
```

{% endraw %}

> django debugger
>
> javascript나 html에서 오류가 나면, 오탈자도 알아채기가 힘들어 장고용 디버거를 설치했다.
> `pip install django-debug-toolbar`
> 자세한 설정은 [이 사이트](https://morningbird.tistory.com/10)를 참고하였음.
> 장고 4.0 이상에서는 `url(r'^__debug__/', include(debug_toolbar.urls)),`를 `path('__debug__/', include(debug_toolbar.urls)),`로 바꿔 주어야 한다.
>
> {% raw %}

```python
import logging
logging.debug('Debug Message')
if some_error:
    logging.error('Error Message')
```

{% endraw %}

## 검색

`distinct`는 조회 결과에 중복이 있을 경우 중복을 제거하여 리턴하는 함수

여러 파라미터를 조합하여 게시물 목록을 조회할 때는 GET 방식을 사용하는 것이 좋다.

question_list.html

{% raw %}

```html
<div class="row my-3">
  <div class="col-6">
    <a href="{% url 'pybo:question_create' %}" class="btn btn-primary"
      >질문 등록하기</a
    >
  </div>
  <div class="col-6">
    <div class="input-group">
      <input
        type="text"
        id="search_kw"
        class="form-control"
        value="{{ kw|default_if_none:'' }}"
      />
      <div class="input-group-append">
        <button class="btn btn-outline-secondary" type="button" id="btn_search">
          찾기
        </button>
      </div>
    </div>
  </div>
</div>
... (중략) ...

<form id="searchForm" method="get" action="{% url 'index' %}">
  <input type="hidden" id="kw" name="kw" value="{{ kw|default_if_none:'' }}" />
  <input type="hidden" id="page" name="page" value="{{ page }}" />
</form>
... (중략) ...
<a
  class="page-link"
  data-page="?page={{ question_list.previous_page_number }}"
  href="javascript:void(0)"
  >이전</a
>
```

{% endraw %}

page, kw를 동시에 요청할 수 있는 자바스크립트 추가

question_list.html

{% raw %}

```html
{% block script %}
<script type="text/javascript">
  const page_elements = document.getElementsByClassName("page-link");
  Array.from(page_elements).forEach(function (element) {
    element.addEventListener("click", function () {
      document.getElementById("page").value = this.dataset.page;
      document.getElementById("searchForm").submit();
    });
  });
  const btn_search = document.getElementById("btn_search");
  btn_search.addEventListener("click", function () {
    document.getElementById("kw").value =
      document.getElementById("search_kw").value;
    document.getElementById("page").value = 1;
    document.getElementById("searchForm").submit();
  });
</script>
{% endblock %}
```

{% endraw %}

base_views.py

{% raw %}

```python
from django.core.paginator import Paginator
from django.shortcuts import render, get_object_or_404
from django.db.models import Q
from ..models import Question


def index(request):
    page = request.GET.get('page', '1')
    kw = request.GET.get('kw', '')
    question_list = Question.objects.order_by('-create_date')
    if kw:
        question_list = question_list.filter(
            Q(subject__icontains=kw) |  # 제목 검색
            Q(content__icontains=kw) |  # 내용 검색
            Q(answer__content__icontains=kw) |  # 답변 내용 검색
            Q(author__username__icontains=kw) |  # 질문 글쓴이 검색
            Q(answer__author__username__icontains=kw)  # 답변 글쓴이 검색
        ).distinct()
    paginator = Paginator(question_list, 10)
    page_obj = paginator.get_page(page)
    context = {'question_list': page_obj, 'page': page, 'kw': kw}
    return render(request, 'pybo/question_list.html', context)

```

{% endraw %}

#### render vs. redirect

render: (템플릿, 컨텍스트) => HttpResponse(status=200, HTML) 을 만들어 내는 함수
redirect: HttpResponseRedirect(status=302, url=URL)을 만들어내는 함수

`render`는 템플릿을 불러 오고, `redirect`는 URL로 이동한다. URL로 이동하므로, 그 URL에 맞는 views가 다시 실행될테고 여기서 render 를 할지 다시 redirect 할지 결정한다.

render 함수는 우리가 넘겨준 request와 template을 토대로 하나의 HttpResponse객체를 리턴해준다.

HTTP 응답 메시지에는 상태 코드(status)라는 코드가 들어갑니다. 100, 200, 300, 400, 500 같은 세 자리 수로 되어 있고 각 코드마다 의미가 있어요
200은 정상적인 결과를 의미하고, 302는 다른 URL을 다시 로드하라고 안내하는 것을 의미합니다.

render와 redirect는 둘 다 HTTP 응답 객체를 만들어내는데요,
render는 상태코드 200과 HTML을 넣은 응답을 생성하고
redirect는 상태코드 302와 참조해야하는 다른 URL을 넣은 응답을 생성합니다.

#### form vs. template

form은 HTML <FORM> 태그를 생성하고, 사용자가 입력한 내용이 올바른지 검사하는 데 쓰입니다.

<FORM> 태그는 사용자가 웹사이트에 정보를 입력하기 위한 양식을 출력하는 태그입니다.

template은 일반적인 HTML 문서를 생성하는 템플릿입니다.

---
layout: post
title:  "점프 투 장고 공부 5"
date:   2022-08-21 22:39:25 +0900
categories: jumptodjango
---

## 3장 파이보 서비스 개발!

### 3-01 내비게이션바

navbar.html

{% raw %}

```html
<!-- 내비게이션바 start -->
<nav class="navbar navbar-expand-lg navbar-light bg-light">
   <div class="container-fluid">
       <a class="navbar-brand" href="{% url 'pybo:index' %}">Pybo</a>
       <button class="navbar-toggler" type="button"
               data-bs-toggle="collapse"
               data-bs-target="#navbarSupportedContent"
               aria-controls="navbarSupportedContent"
               aria-expanded="false"
               aria-label="Toggle navigation">
           <span class="navbar-toggler-icon"></span>
       </button>
       <div class="collapse navbar-collapse" id="navbarSupportedContent">
           <ul class="navbar-nav me-auto mb-2 mb-lg-0">
               <li class="nav-item">
                   <a class="nav-link" href="#">로그인</a>
               </li>
           </ul>
       </div>
   </div>
</nav>

<!-- 내비게이션바 end -->
```
{% endraw %}



navbar.html 파일은 다른 템플릿들에서 중복되어 사용되지는 않지만 독립된 하나의 템플릿으로 관리하는 것이 유지 보수에 유리하므로 분리하였다.


views.py

{% raw %}

```python
from django.core.paginator import Paginator

from .models import Question
from .forms import QuestionForm, AnswerForm


def index(request):
    page = request.GET.get('page', '1')
    question_list = Question.objects.order_by('-create_date')
    paginator = Paginator(question_list, 10)
    page_obj = paginator.get_page(page)
    context = {'question_list': page_obj}
    return render(request, 'pybo/question_list.html', context)
```
{% endraw %}



`page = request.GET.get('page', '1')`은 http://localhost:8000/pybo/?page=1 처럼 GET 방식으로 호출된 URL에서 page값을 가져올 때 사용한다. 

`page_obj = paginator.get_page(page)`
`paginator`를 이용하여 요청된 페이지(page)에 해당되는 페이징 객체(`page_obj`)를 생성했다. 이렇게 하면 장고 내부적으로는 데이터 전체를 조회하지 않고 해당 페이지의 데이터만 조회하도록 쿼리가 변경된다.


> HTML5
> * `<ul>` 순서 없는 목록(Unordered List)이란 나열된 항목들 간의 구분이 없는 목록을 뜻합니다. 
> * `<ol>` 순서 있는 목록(Ordered List)이란 기호를 이용해서 각 항목들 사이의 순서를 구분하는 목록을 뜻합니다.



페이징 작업

question_list.html

{% raw %}
```html
    <!-- 페이징 시작 -->

        <!-- 페이지 리스트 시작 -->
        {% for page_number in question_list.paginator.page_range %}
        {% if page_number >= question_list.number|add:-5 and page_number <= question_list|add:5 %}<!-- 현재 페이지 기준으로 좌우 5개씩 보이도록 만든다. -->
        {% if page_number == question_list.number %}
        <li class="page-item active" aria-current="page">
            <a class="page-link" href="?page={{ page_number }}">{{ page_number }}</a>
        </li>
        {% else %}
        <li class="page-item">
            <a class="page-link" href="?page={{ page_number }}">{{ page_number }}</a>
        </li>
        {% endif %}
        {% endif %}
        {% endfor %}
        <!-- 페이지 리스트 끝 -->
        <!-- 다음 페이지 시작 -->
        {% if question_list.has_next %}
        <li class="page-item">
            <a class="page-link" href="?page={{ page_number }}">다음</a>
        </li>
        {% else %}
        <li class="page-item disabled">
            <a class="page-link" tabindex="-1" aria-disabled="true" href="#">다음</a>
        </li>
        {% endif %}
        <!-- 다음 페이지 끝 -->
    </ul>
    <!-- 페이징 끝 -->
```

{% endraw %}



### 3-03 템플릿 필터

페이지별로 게시물의 번호를 역순으로 정렬하려면 다음과 같은 공식을 적용해야 한다.

* 번호 = 전체건수 - 시작인덱스 - 현재인덱스 + 1

장고에는 빼기 필터가 없으므로 직접 만든다. `@register.filter` 애너테이션을 적용하면 템플릿에서 해당 함수를 필터로 사용할 수 있다.

pybo_filter.py

{% raw %}
```python
from django import template

register = template.Library()


@register.filter
def sub(value, arg):
    return value - arg
```
{% endraw %}


글 번호 순서대로 잘 나오게 하기

question_list.html

{% raw %}
```html
        {% if question_list %}
        {% for question in question_list %}
            <tr>
                <td>
                    <!-- 번호 = 전체건수 - 시작인덱스 - 현재인덱스 + 1 -->
                    {{ question_list.paginator.count|sub:question_list.start_index|sub:forloop.counter0|add:1 }}
                </td>
                <td>
                    <a href="{% url 'pybo:detail' question.id %}">{{ question.subject }}</a>
                </td>
                <td>{{ question.author }}</td>
                <td>{{ question.create_date }}</td>
            </tr>
        {% endfor %}
        {% else %}
        <tr>
            <td colspan="3">질문이 없습니다.</td>
        </tr>
        {% endif %}
```
{% endraw %}



> django 코드 github.io 블로그 글 업로드 에러
> * django 코드가 포함된 블로그 글이 업로드되지 않았다. 지킬 태그로 인식하여 오류가 나는 것이었다. 이를 방지하려면 \{% raw %\} \{% endraw %\}로 코드를 감싸면 된다.

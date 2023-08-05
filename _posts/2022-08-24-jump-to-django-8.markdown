---
layout: post
title: "점프 투 장고 공부 8"
date: 2022-08-24 22:25:11 +0900
tags: jumptodjango
---

### 글쓴이 항목 추가

models.py
{% raw %}

```python
from django.db import models
from django.contrib.auth.models import User

class Question(models.Model):
    subject = models.CharField(max_length=200)
    content = models.TextField()
    create_date = models.DateTimeField()
    author = models.ForeignKey(User, on_delete=models.CASCADE)

    def __str__(self):
        return self.subject


class Answer(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    content = models.TextField()
    create_date = models.DateTimeField()
    author = models.ForeignKey(User, on_delete=models.CASCADE)
```

{% endraw %}

`on_delete=models.CASCADE`는 계정이 삭제되면 이 계정이 작성한 질문을 모두 삭제하라는 의미

임의로 작성자를 추가한 적이 있다. 이번 실습 조건과 충돌이 있어 오류가 났다.

1. 지난번에 텍스트 타입으로 author 칼럼을 만들었던 마이그레이션 스크립트를 삭제했다.
   그 이후에 생성된 스크립트들도 같이 삭제한다.
2. 데이터베이스에 있는 author filed를 삭제해서 원래대로 돌렸다.
3. 그리고 makemigration 을 해서 스크립트를 새로 만들었다.

views.py
{% raw %}

```python
@login_required(login_url='common:login')
def answer_create(request, question_id):
    question = get_object_or_404(Question, pk=question_id)

    if request.method == 'POST':
        form = AnswerForm(request.POST)
        if form.is_valid():
            answer = form.save(commit=False)
            answer.create_date = timezone.now()
            answer.question = question
            answer.author = request.user
            answer.save()
            return redirect('pybo:detail', question_id=question.id)
    else:
        return HttpResponseNotAllowed('only POST is allowed')
    context = {'question': question, 'form': form}
    return render(request, 'pybo/question_detail.html', context)


@login_required(login_url='common:login')
def question_create(request):
    if request.method == 'POST':  # '질문 등록' 화면에서 [저장하기] 버튼을 누른 경우 -> 질문 내용을 데이터베이스에 저장해야 함.
        form = QuestionForm(request.POST)
        # request.POST를 인수로 QuestionForm을 생성할 경우에는
        # request.POST에 담긴 subject, content 값이 QuestionForm의 subject, content 속성에 자동으로 저장되어 객체가 생성된다.
        if form.is_valid():
            question = form.save(commit=False)  # 임시저장
            question.create_date = timezone.now()
            question.author = request.user
            question.save()
            return redirect('pybo:index')
    else:  # '질문 목록' 화면에서 직접 접속한 경우
        form = QuestionForm()
    context = {'form': form}
    return render(request, 'pybo/question_form.html', context)
```

{% endraw %}

`request.user`는 현재 로그인한 계정의 User 모델 객체이다.

`request.user`에는 로그아웃 상태이면 `AnonymousUser` 객체가, 로그인 상태이면 `User` 객체가 들어있는데,

`@login_required` 애너테이션이 붙은 함수는 로그인이 필요한 함수를 의미한다.

로그아웃 상태에서 `@login_required` 어노테이션이 적용된 함수가 호출되면 자동으로 로그인 화면으로 이동하게 된다.

login.html
{% raw %}

```html
{% extends "base.html" %} {% block content %}
<div class="container my-3">
  <form method="post" action="{% url 'common:login' %}">
    {% csrf_token %}
    <input type="hidden" name="next" value="{{ next }}" />
    {% include "form_errors.html" %}
    <div class="mb-3">
      <label for="username">사용자 ID</label>
      <input
        type="text"
        class="form-control"
        id="username"
        name="username"
        value="{{ form.username.value|default_if_none:'' }}"
      />
    </div>
    <div class="mb-3">
      <label for="password">비밀번호</label>
      <input type="password" class="form-control" id="password" name="password"
      value="{{ form.password.value|default_if_none:'' }}"
    </div>
    <button type="submit" class="btn btn-primary">로그인</button>
  </form>
</div>
{% endblock %}
```

{% endraw %}

question_detail.html
{% raw %}

```html
<div class="mb-3">
  <label for="content" class="form-label">답변 내용</label>
  <textarea
    {%
    if
    not
    user.is_authenticated
    %}disabled{%
    endif
    %}
    name="content"
    id="content"
    class="form-control"
    rows="10"
  ></textarea>
</div>
```

{% endraw %}

### html 화면에 글쓴이 추가

question_list.html
{% raw %}

```html
<tr class="text-center table-dark">
  <th>번호</th>
  <th style="width:50%">제목</th>
  <th>글쓴이</th>
  <th>작성 일시</th>
</tr>
```

{% endraw %}

제목의 너비가 전체에서 50%를 차지하도록

question_detail.html
{% raw %}

```html
<div class="badge bg-light text-dark p-2 text-start">
  <div class="mb-2">{{ question.author.username }}</div>
  <div>{{ answer.create_date }}</div>
</div>
```

{% endraw %}

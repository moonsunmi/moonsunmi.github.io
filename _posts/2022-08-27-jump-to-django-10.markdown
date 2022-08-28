---
layout: post
title:  "점프 투 장고 공부 10"
date:   2022-08-27 22:00:55 +0900
categories: jumptodjango
---


#### 질문 수정

views.py
{% raw %}
```python

@login_required(login_url='common:login')
def question_modify(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    if request.user != question.author:
        messages.error(request, '수정권한이 없습니다.')
        return redirect('pybo:detail', question_id=question.id)
    if request.method == 'POST':
        form = QuestionForm(request.POST, instance=question)
        if form.is_valid():
            question = form.save(commit=False)
            question.modify_date = timezone.now()
            question.save()
            return redirect('pybo:detail', question_id=question.id)
    else:
        form = QuestionForm(instance=question)
    context = {'form': form}
    return render(request, 'pybo/question_form.html', context)
```
{% endraw %}


#### 질문 삭제

views.py
{% raw %}
```python

@login_required(login_url='common:login')
def question_delete(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    if request.user != question.author:
        messages.error(request, '수정 권한이 없습니다.')
        return redirect('pybo:detail', question_id=question.id)
    question.delete()
    return redirect('pybo:index')


```
{% endraw %}

question_detail.html

{% raw %}
```html
{% block script %}
<script type="text/javascript">
const delete_elements = document.getElementsByClassName("delete");
Array.from(delete_elements).forEach(function(element) {
    element.addEventListener('click', function() {
        if(confirm("정말로 삭제하시겠습니까?")) {
            location.href = this.dataset.uri;
        };
    });
});
</script>
{% endblock %}

```
{% endraw %}

javascript는 `</body>` 바로 위에 넣어 주는 걸 추천.


#### 답변 수정


답변 수정의 form은 새로 만들어 주어야 한다.

answer_form.html

{% raw %}
```html
{% extends 'base.html' %}
{% block content %}
<!-- 답변 수정 시작 -->
<div class="container my-3">
    <form method="post">
        {% csrf_token %}
        {% include "form_errors.html" %}
        <div class="mb-3">
            <label for="content" class="form-label">답변내용</label>
            <textarea class="form-control" name="content" id="content" rows="10">
                {{ form.content.value|default_if_none:'' }}
            </textarea>
        </div>
        <button type="submit" class="btn btn-primary">저장하기</button>
    </form>
</div>
<!-- 답변 수정 끝 -->

{% endblock %}
```
{% endraw %}



답변을 수정하거나 삭제할 경우에도, detail 화면으로 넘어간다. 그런데 여기에서 detail 화면은 질문을 기준으로 보여주는 것이기 때문에 detail 화면으로 넘어갈 때, `question_id` 값을 넘겨 주어야 한다.
`answer` 객체를 이용해 `question_id` 값을 구하는 방법: `answer.question.id`

{% raw %}
```python

@login_required(login_url='common:login')
def answer_modify(request, answer_id):
    answer = get_object_or_404(Answer, pk=answer_id)

    if request.user != answer.author:
        messages.error(request, '수정 권한이 없습니다.')
        return redirect('pybo:detail', question_id=answer.question.id)

    if request.method == 'POST':
        form = AnswerForm(request.POST, instance=answer)
        if form.is_valid():
            answer = form.save(commit=False)
            answer.modify_date = timezone.now()
            answer.save()

        return redirect('pybo:detail', question_id=answer.question.id)
    else:
        form = AnswerForm(instance=answer)
    context = {'answer': answer, 'form': form}
    return render(request, 'pybo/answer_form.html', context)

```
{% endraw %}


#### 답변 삭제


{% raw %}
```python
@login_required(login_url='common/login')
def answer_delete(request, answer_id):
    answer = get_object_or_404(Answer, pk=answer_id)

    if request.user != answer.author:
        messages.error(request, '권한이 없습니다.')

    answer.delete()
    return redirect('pybo:detail', question_id=answer.question.id)
```
{% endraw %}



### views.py 파일 분리

views 파일의 내용이 너무 많아져서, 분리가 필요하다.

> control+alt+o: 안 쓰는 import 자동 삭제

1. views.py만 여러 파일로 구분

views 디렉터리를 만들고 그 안에 views 파이를 세 개 만들었다.
기능별로, base_views.py, question_views.py, answer_views.py로 나누었다.

디렉터리 한 단계 내려 갔기 때문에 `from .model` 등으로 된 것은 `from ..model`으로 변경


2. 1번 방법 개선하기

pybo/urls.py

{% raw %}
```python
from django.urls import path
from .views import base_views, question_views, answer_views

app_name = 'pybo'

urlpatterns = [
    # base_views.py
    path('', base_views.index, name='index'),
    path('<int:question_id>/', base_views.detail, name='detail'),

    # answer_views.py
    path('answer/create/<int:question_id>/', answer_views.answer_create, name='answer_create'),
    path('answer/modify/<int:answer_id>/', answer_views.answer_modify, name='answer_modify'),
    path('answer/delete/<int:answer_id>/', answer_views.answer_delete, name='answer_delete'),

    # question_views.py
    path('question/create/', question_views.question_create, name='question_create'),
    path('question/modify/<int:question_id>/', question_views.question_modify, name='question_modify'),
    path('question/delete/<int:question_id>/', question_views.question_delete, name='question_delete'),
]
```
{% endraw %}

config/urls.py

{% raw %}
```python
from django.contrib import admin
from django.urls import path, include
from pybo.views import base_views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('pybo/', include('pybo.urls')),
    path('common/', include('common.urls')),
    path('', base_views.index, name='index')
]
```
{% endraw %}



## 3-11 추천

한 질문에 여러 사람이 추천 가능.
한 사람이 여러 질문에 추천 가능.
다대다 관계에서는 `ManyToManyField`. ForeignKey처럼 동작.


### 질문 추천

특정 사용자가 작성한 질문을 얻기 위해서는 `some_user.author_question.all()`
특정 사용자가 추천한 질문을 얻기 위해서는 `some_user.voter_question.all()`


question_detail.html
{% raw %}
```html
<a href="javascript:void(0)" data-uri="{% url 'pybo:question_vote' question.id  %}"
   class="recommend btn btn-sm btn-outline-secondary"> 추천
    <span class="badge rounded-pill bg-success">{{question.voter.count}}</span>
</a>
... (중략) ...
const recommend_elements = document.getElementsByClassName("recommend");
Array.from(recommend_elements).forEach(function(element) {
    element.addEventListener('click', function() {
        if(confirm("정말로 추천하시겠습니까?")) {
            location.href = this.dataset.uri;
        };
    });
});

```
{% endraw %}


동일한 사용자가 동일한 질문을 여러 번 추천하더라도 추천수가 증가하지는 않는다. `ManyToManyField` 내부에서 자체적으로 처리된다.


### 답변 추천


question_detail.html
{% raw %}
```html
<a href="javascript:void(0)" data-uri="{% url 'pybo:answer_vote' answer.id %}" class="recommend btn btn-sm btn-outline-secondary">추천
    <span class="badge rounded-pill bg-success">{{ answer.voter.count }}</span>
</a>
```
{% endraw %}

javascript는 질문 추천의 것을 쓸 수 있으므로 작성할 필요가 없다.


answer_views.py

{% raw %}
```python

@login_required(login_url='common/login')
def answer_vote(request, answer_id):
    answer = get_object_or_404(Answer, pk=answer_id)

    if request.user == answer.author:
        messages.error(request, '자신의 댓글을 추천할 수 없습니다.')
    else:
        answer.voter.add(request.user)

    return redirect('pybo:detail', question_id=answer.question.id)

```

{% endraw %}

---
layout: post
title: "파이썬 웹 프로그래밍 공부 4 & 3장까지의 리뷰"
date: 2022-08-16 22:08:52 +0900
tags: python-web-programming
---

#### View, Template 코딩

뷰와 템플릿은 서로 영향을 미치기 때문에 보통 같이 작업한다. 템플릿 먼저 하길 추천.

템플릿(index.html)
{% raw %}

```html
{% if latest_question_list %}
<ul>
  {% for question in latest_question_list %}
  <li><a href="/polls/{{ question.id}}/">{{ question.question_text }}</a></li>
  {% endfor %}
</ul>
{% else %}
<p>No polls are available!</p>
{% endif %}
```

{% endraw %}

뷰(views.py)
{% raw %}

```python
from django.shortcuts import render
from polls.models import Question

def index(request):

   latest_question_list = Question.objects.all().order_by('-pub_date')[:5]
   context = {'latest_question_list', latest_question_list}
   return render(request, 'polls/index.html', context)
```

`render()` 함수는 템플릿 파일인 polls/index.html에 `context` 변수를 적용해 사용자에게 보여줄 최종 HTML 텍스트를 만들고, 이를 담아서 `HttpResponse` 객체를 반환한다.
`context`는 사전 형태로.

detail.html
{% raw %}

```html
<h1>{{question.question_text}}</h1>

{% if error_message %}
<p><strong>{{ error_message }}</strong></p>
{% endif %}

<form action="{% url 'polls:vote' question.id %}" method="post">
  {% csrf_token %} {% for choice in question.choice_set.all %}
  <input
    type="radio"
    name="choice"
    id="choice{{ forloop.counter }}"
    value="{{ choice.id }}"
  />
  <label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label
  ><br />
  {% endfor %}
  <input type="submit" value="Vote" />
</form>
```

{% endraw %}

`polls:vote`: URLconf에서 정의한 URL 패턴 이름

서버 측의 데이터를 변경하는 경우 폼은 POST 방식으로..

`csrf_token`: CSRF 공격을 방지하기 위한 기능. 폼 처리할 때 해야 함.

input id와 submit for 속성이 같아야 서로 바인딩된다.

`<input>` 태그의 `name`과 `value` 속성값이 `request.Post` 사전에 `key`, `value`로 사용된다.

- `Question`과 `Choice` 테이블의 관계는 1:N. 이 관계에서 `xxx_set` 속성을 디폴트로 제공한다.

`detail()` 뷰 함수에서 정의해야 할 `context`가 무엇이 있는지 detail.html에서 찾아본다. question.id, question.text … 전부 question만 있으면 됨..!

```python

def detail(request, question_id):
   question = get_object_or_404(Question, pk=question_id)
   return render(request, 'polls/detail.html', {'question' : question})
```

- 리다이렉트: 넘겨받은 URL을 웹 브라우저가 열려고 하면, 다른 문서의 URL이 열린다.

- `request.POST`: 제출된 폼의 데이터를 담고 있는 객체, 파이썬 사전처럼 키로 그 값을 구할 수 있다.

- Post 방식의 폼 데이터를 처리하는 경우, 결과를 보여줄 수 있는 페이지로 이동시키기 위해 리다이렉트 객체를 리턴하는 것이 일반적이다.

`reverse(): 
/polls/3/results/   —-(뷰 함수 매핑)--->    views.results()
views.results()   —(reverse(‘polls:results’, args=3,) URL 추출)    —> polls/3/results/`

`results()` 뷰 함수의 호출과 연계된 URL은 `votes()` 뷰 함수의 리다이렉트 결과로 받습니다.

results.hml
{% raw %}

```html
<h1>{{ question.question_text }}</h1>

<ul>
  {% for choice in question.choice_set.all %}
  <li>
    {{ choice.choice_text }} - {{ choice.votes }} vote{{ choice.votes|pluralize
    }}
  </li>
  {% endfor %}
</ul>

<a href="{% url 'polls.detail' question.id %}">Vote again?</a>
```

{% endraw %}

result()

```html
def results(request, question_id): question = get_object_or_404(Question,
pk=question_id) return render(request, 'polls/results.html', {'question':
question})
```

## <파이썬 웹 프로그래밍> 1~3장 공부 리뷰

1~2장은 너무 어렵다.

지금 내가 실습하고 있는 코드가 어떤 일을 하고 있는 건지, 그게 어떤 결과에 영향을 미치는지(그래서 결과물에서 어떤 점을 확인해야 하는지) 등이 안 나와 있어서 감이 안 잡혔다. 코드와 용어에 익숙해진다는 생각으로 책을 읽고 실습해 나갔다.

이 코드가 어떤 일을 하고 있고, 결과가 어떤 걸 보여준다는 건지 독자들이 다 알고 있다는 전제하에 진행되는 것 같다. 스트레스 많이 받고, 너무 포기하고 싶어짐.

내가 도대체 뭘 하고 있는 걸까? 방식 하나씩 다 따라 해 보는 거라서 그런지, 결과물과 매칭도 잘 안 되고… 이거 계속 한다고 머리에 뭐가 남는 건 있을까?

3장부터는 좀 쉬워진다. 2장의 위치가 뒤에 있었으면 이해가 좀 더 잘 되었을 거 같다.
여튼 좀 어려운 것 같아서, <점프투 장고>를 먼저 보기로 했다. <점프투 장고> 공부를 다 끝난 후에 나머지 장을 볼 예정이다.(배포 전까지)

프레임워크를 처음 써 보는데, 편리함과 장점이 참 많다.

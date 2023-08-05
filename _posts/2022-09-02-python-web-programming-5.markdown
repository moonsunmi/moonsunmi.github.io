---
layout: post
title: "파이썬 웹 프로그래밍 공부 5"
date: 2022-09-02 22:15:44 +0900
tags: python-web-programming
---

## 13장 Model

models.py 파일에는 테이블을 정의하는 것이 기본.
ORM 방식에서는 관련 변수 메소드를 추가적으로 정의할 수 있는 장점이 있다.

테이블의 컬럼(장고에서는 테이블의 필드 또는 모델 필드라고 함)은 모델 클래스의 속성으로 정의, 테이블과 관련된 함수는 모델 클래스의 메소드로 정의

#### 마이그레이션

데이터베이스의 테이블 및 필드에서 생긴 변경사항을 알려주는 정보이다.
makemigrations 명령으로 polls/migrations 디렉토리 하위에 마이그레이션 파일들이 생긴다.

#### import 문법

polls
ㄴ models.py
ㄴ admin.py

`from polls import models.Question` 형태를 추천한다.

#### path()

path(route, view, kwargs, name)
`path('<int:question_id>/vote/', views.vote, name='vote')`

- (필수) route: URL 패턴을 표현하는 문자열. URL 스트링이라고 부른다.
- (필수) view: URL 스트링이 매칭되면 호출되는 뷰 함수. HttpRequest 객체(request)와 URL 스트링에서 추출된 항목(`<int:question_id>`의 `question_id`)이 뷰 함수의 인자로 전달된다.
- kwargs: 추가적인 인자를 뷰 함수에 전달할 때, 사전 타입으로 정의한다.
- name: 패턴의 이름. 템플릿 파일에서는 이 이름을 주로 사용한다.

mysite/setting.py 파일에 `ROOT_URLCONF = 'mysite.urls'`라고 되어 있기 때문에 URL 분석 시 urls.py 파일을 가장 먼저

index.html 파일을 작성하면서, 어떤 변수가 필요한지 확인하는 게 중요하다. 그 변수들은 view 함수에서 context로 받아온다.0

{% raw %}

```html
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

폼 데이터는 POST 방식으로 polls:vote URL로 전송된다. vote() 뷰 함수에서 request.POST['choice'] 구문으로 액세스한다. `<input>` 태그의 name과 value 속성값들이 request.POST 사전에 key, value로 이용된다.

`question.choice_set.all` : Question 테이블의 question 레코드에 연결된 Choice 테이블의 레코드 모두. 템플릿 문법상 메서드 호출을 표시하는 ()를 사용하지 않으므로 .all로만 표시됨.

POST 방식의 폼 데이터를 처리하는 경우, 그 결과를 보여주 수 있는 페이지로 이동시키기 위해 Redirect 객체를 리턴하는 것이 일반적이다.

reverse(): URL 패턴명으로부터 URL 스트링을 구한다.

## 4장

모델과 장고의 admin 계정은 서로 연관이 있어서
모델을 수정해 장고의 admin을 바꿀 수도, 장고의 admin을 수정해 모델을 바꿀 수도 있다.

admin.py
{% raw %}

```python
from django.contrib import admin
from polls.models import Question, Choice


class ChoiceInline(admin.TabularInline):
    model = Choice
    extra = 2


class QuestionAdmin(admin.ModelAdmin):
    fieldsets = [
        (None, {'fields': ['question_text']}),
        ('Date information', {'fields': ['pub_date'], 'classes': ['collapse']}),
    ]
    inlines = [ChoiceInline]
    list_display = ('question_text', 'pub_date')
    list_filter = ['pub_date']
    search_fields = ['question_text']


admin.site.register(Question, QuestionAdmin)
admin.site.register(Choice)


```

{% endraw %}

- Admin 사이트 템플릿 안 됨..ㅠ

## 장고 파이썬 셸

manage.py 모듈에서 정의한 DJANGO_SETTINGS_MODULES 속성을 이용해 미리 mysite/settings.py 모듈을 임포트한다.

### CREATE

{% raw %}

```python
>>> from polls.models import Question, Choice
>>> from django.utils import timezone
>>> q = Question(question_text="What's new?", pub_date=timezone.now())
>>> q.save()
>>>

```

{% endraw %}

### READ

QuerySet 객체: 데이터베이스 테이블로부터 꺼내 온 객체들의 콜렉션(SQL의 SELECT 문장) 필터로(SQL의 WHERE 절) 조건에 맞는 레코드만 다시 추출할 수 있다.

objects 객체: 테이블 정보를 담고 있는 객체

QuerySet 메서드들은 QuerySet 컬렉션을 반환하므로, 체인식 호출이 가능하다.

한 개의 요소만 있는 게 확실하면 get() 메서드 이용한다.

### UPDATE

하나의 수정은 대입문으로.
한꺼번에 여러 객체를 수정할 경우 update() 메서드

### DELETE

delete() 메서드

### 실습

{% raw %}

```python
>>> Question.objects.filter(question_text__startswith='What')
<QuerySet [<Question: What is your hobby ?>, <Question: What do you like best ?>, <Question: What's new ?>, <Question: What's new?>, <Question: What's new~~?>]>

>>> q = Question.objects.get(pk=2)
>>> q.choice_set.all()
<QuerySet [<Choice: Eating>, <Choice: Playing>]>

>>> q.choice_set.create(choice_text='Sleeping', votes=0)
<Choice: Sleeping>
>>> q.choice_set.create(choice_text='Eating', votes=0)
<Choice: Eating>
>>> c = q.choice_set.create(choice_text='Playing', votes=0)
>>> c.question
<Question: What do you like best ?>
>>> q.choice_set.all()
<QuerySet [<Choice: Eating>, <Choice: Playing>, <Choice: Sleeping>, <Choice: Eating>, <Choice: Playing>]>
>>> q.choice_set.count()
5


>>> from django.utils import timezone
>>> current_year = timezone.now().year
>>> Choice.objects.filter(question__pub_date__year=current_year)
<QuerySet []>
>>> c = q.choice_set.filter(choice_text__startwswith='Sleeping')
>>> c = q.choice_set.filter(choice_text__startswith='Sleeping')
>>> c.delete()
(1, {'polls.Choice': 1})
>>> Choice.objects.all()
<QuerySet [<Choice: Reading>, <Choice: Soccer>, <Choice: Climbing>, <Choice: Seoul>, <Choice: Daejeon>, <Choice: Jeju>, <Choice: Eating>, <Choice: Playing>, <Choice: Eating>, <Choice: Playing>]>
```

{% endraw %}

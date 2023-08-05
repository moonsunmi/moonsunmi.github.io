---
layout: post
title: "점프 투 장고 공부 6"
date: 2022-08-22 22:12:22 +0900
tags: jumptodjango
---

### 3-04 답변 개수 표시

question_list.html

{% raw %}

```html
<td>
  <a href="{% url 'pybo:detail' question.id %}">{{ question.subject }}</a>
  {% if question.answer_set.count > 0 %}
  <span class="text-danger small mx-2">{{ question.answer_set.count }}</span>
  {% endif %}
</td>
```

{% endraw %}

### 3-05 로그인과 로그아웃

장고의 로그인, 로그아웃을 도와주는 앱은 `django.contrib.auth`
setting.py에 자동생성됨.

로그인,로그아웃은 앱 공통의 기능이므로, 이에 해당하는 common 앱을 만듦.

`(mysite) mysite % django-admin startapp common`

mysite의 config의 settings.py에 방금 만든 앱을 등록, urls.py에 해당 URL도 세팅해 줌.

LoginView는 registraion 템플릿 디렉터리에서 login.html 파일을 찾는다. 여기서는 common 디렉터리에서 해야 하므로,

`path('login/', auth_views.LoginView.as_view(template_name='common/login.html'), name='login')`

login.html
{% raw %}

```html
{% extends "base.html" %} {% block content %}
<div class="container my-3">
  <form method="post" action="{% url 'common:login' %}">
    {% csrf_token %} {% include "form_error.html" %}
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

form_error.html(얘는 common 앱 안에 소속되지 않고, mysite/templates 안에 넣었다.)
{% raw %}

```html
<!-- 필드 오류와 넌필드 오류를 출력한다. 시작 -->
{% if form.errors %}
<div class="alert alert-danger">
  <!-- 필드 오류 시작 -->
  {% for field in form %} {% if field.errors %}
  <div>
    <stron>{{ field.label }}</stron>
    {{ field.errors }}
  </div>
  {% endif %} {% endfor %}
  <!-- 필드 오류 끝 -->
  <!-- 넌필드 오류 시작 -->
  {% for error in field.non_field_errors %}
  <div>
    <strong>{{ error }}</strong>
  </div>
  {% endfor %}
  <!-- 넌필드 오류 끝 -->
</div>
{% endif %}
<!-- 필드 오류와 넌필드 오류를 출력한다. 끝 -->
```

{% endraw %}

common/urls.py
{% raw %}

```python
from django.urls import path
from django.contrib.auth import views as auth_views

app_name = 'common'

urlpatterns=[
    path('login/', auth_views.LoginView.as_view(template_name='common/login.html'), name='login'),
    path('logout/', auth_views.LogoutView.as_view(), name='logout'),
]
```

{% endraw %}

## HTML

### `<form>`

웹 이용자가 입력 요소에 입력한 값들을 서버로 전송해주는 역할을 하는 '폼 요소(양식)'를 만드는 태그. 폼 요소가 역할을 수행하는 방식이나 처리할 작업 등을 지정하는 몇 가지 속성들을 추가할 수 있습니다.

- action : 폼 요소에 입력된 값들을 전달받아 처리할 서버의 프로그램을 지정합니다(URL).
- method : 사용자가 입력한 내용들을 서버로 전송할 때의 방식을 지정합니다.

### `<label>`

입력 요소에 라벨을 붙이는 역할

1. 태그의 안에 입력 요소를 포함시키는 방식
   {% raw %}

```html
<label>
  아이디
  <input type="text" />
</label>
```

{% endraw %}

2. 입력 요소의 id를 기반으로 <label> 태그와 입력 요소를 짝 지어주는 방식
   {% raw %}

```html
<label for="pw">비밀번호</label> <input id="pw" type="password" />
```

{% endraw %}

### HTML `name` 속성

요소 안에 포함되는 각 입력 요소에는 name 속성을 추가하여 각 입력 항목의 역할을 구별해줄 수 있다.

{% raw %}

```html
<body>
  <form method="post" action="fake_server.php">
    <label for="name">이름</label>
    <input type="text" id="name" name="name" />
    <br />
    <label for="age">나이</label>
    <input type="number" id="age" name="age" />
    <br />
    <label for="character">성격</label>
    <textarea id="character" name="character"></textarea>
    <br />
    <input type="button" value="전송" />
  </form>
</body>
```

{% endraw %}

- id나 class가 있는데, 왜 또 name이 들어 갔을까?
  각 입력 요소에 지정된 name 속성은 폼 안에서 입력 요소를 구분하는 역할을 함과 동시에, 서버에 전송된 입력 값의 이름으로 사용

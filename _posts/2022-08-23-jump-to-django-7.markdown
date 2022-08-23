---
layout: post
title:  "점프 투 장고 공부 7"
date:   2022-08-23 22:22:51 +0900
categories: jumptodjango
---

### 회원가입

계정 생성 시 사용할 User 폼.
폼: 페이지 요청시 전달되는 파라미터들을 쉽게 관리하기 위해 사용하는 클래스이다. 폼은 필수 파라미터의 값이 누락되지 않았는지, 파라미터의 형식은 적절한지 등을 검증할 목적으로 사용한다.

common/forms.py
{% raw %}
```python
from django import forms
from django.contrib.auth.forms import UserCreationForm
from django.contrib.auth.models import User


class UserForm(UserCreationForm):
    email = forms.EmailField(label="이메일")

    class Meta:
        model = User
        fields = ("username", "password1", "password2", "email")
```
{% endraw %}


이메일 항목을 추가하기 위해서 `UserCreationForm`을 상속한 `UserForm`을 만듦.

`UserCreationForm`의 `is_valid` 함수는 폼이 모두 입력되었는지, 비밀번호1과 비밀번호2가 같은지, 비밀번호의 값이 비밀번호 생성 규칙에 맞는지 등을 검사하는 로직을 내부적으로 가지고 있다.

common/views.py
{% raw %}
```python
from django.contrib.auth import authenticate, login
from django.shortcuts import render, redicrect
from common.forms import UserForm


def signup(request):

    if request.method == 'POST':
        # 정보 입력 후 회원 가입 누른 거
        form = UserForm(request.POST)
        if form.is_valid():
            form.save()
            username = form.cleaned_data.get('username')
            raw_password = form.cleaned_data.get('password1')
            user = authenticate(username=username, password=raw_password)
            login(request, user)
            return redicrect('index')
    else:
        form = UserForm()
    return render(request, 'common/signup.html', {'form': form})
```
{% endraw %}


`form.cleaned_data.get` 함수는 폼의 입력값을 개별적으로 얻고 싶은 경우에 사용하는 함수

signup.html
{% raw %}
```html
{% extends "base.html" %}
{% block content %}
<div class="container my-3">
  <form method="post" action = ""{% url 'common:signup' %}">
    {% csrf_token %}
    {% include "form_errors.html" %}
    <div class="mb-3">
      <label for="username">사용자이름</label>
      <input type="text" id="username" class="form-control" name="username" value="{{ form.username.value|default_if_none:'' }}">
    </div>
    <div class="mb-3">
      <label for="password1">비밀번호</label>
      <input type="password" id="password1" class="form-control" name="password1" value="{{ form.password1.value|default_if_none:'' }}">
    </div>
    <div class="mb-3">
      <label for="password2">비밀번호 확인</label>
      <input type="password" id="password2" class="form-control" name="password2" value="{{ form.password2.value|default_if_none:'' }}">
    </div>
    <div class="mb-3">
      <label for="email">이메일</label>
      <input type="text" id="email" class="form-control" name="email" value="{{ form.email.value|default_if_none:'' }}">
    </div>
    <button type="submit" class="btn btn-primary  ">가입하기</button>
  </form>
</div>
{% endblock %}
```
{% endraw %}


* `mb`: maring-buttom. mb-3은 하단에 3칸 공백을 줌. 2로 바꾸면 2칸










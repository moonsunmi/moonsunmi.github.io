---
layout: post
title: "파이썬 웹 프로그래밍 공부 6"
date: 2022-09-03 22:21:38 +0900
tags: python-web-programming
---

### 템플릿 변수

> 장고에서 foo.bar 템플릿 변수의 해석 순서
>
> 1. foo가 사전 타입이면 `foo['bar']`
> 2. foo에 bar라는 속성이 있으면 `foo.bar`
> 3. foo가 리스트면 `foo[bar]`

### 템플릿 필터

필터: 어떤 객체나 처리 결과에 추가적으로 명령을 적용하여 해당 명령에 맞게 최종 결과를 변경하는 것

#### url 태그

{% raw %}

```html
{% url 'namespace:view-name' arg1 arg2 %}
```

{% endraw %}

- namespace: urls.py 파일의 `include()` 함수 또는 `app_name` 변수에 정의한 이름 공간 이름
- view-name: urls.py 파일에서 정의한 URL 패턴 이름
- argN: 뷰 함수에서 사용하는 인자

with: 특정 값을 변수에 저장해 두는 기능.
{% raw %}

```html
{% with total=apple.count %} {{ total }} {% endwith %}
```

{% endraw %}

load: 사용자 정의 필터 로딩
{% raw %}

```html
{% load somlibrary package.otherlibrary %}
```

{% endraw %}

주석: `{# #}`, {% raw %}`{% comment %}`{% endraw %}

### 이스케이프

사용자가 입력한 데이터를 그대로 렌더링하는 것은 위험할 수 있다.
예를 들어 `name="<b>username"`일 때, `{{ name }}`하면, `username`이 볼드처리되어 버릴 수 있다. 그래서 장고는 자동 이스케이프 기능을 제공한다.

자동 이스케이프 기능을 비활성화시키려면 safe 이용한다. `{{ data|safe }}`

{% raw %}

```html
{% autoescape off %} Hello {{ name }} {% endautoescape %}
```

{% endraw %}}

{% raw %}`{% extends "base.html" %}`{% endraw %}의 뜻: base.html 부모 템플릿을 상속받는다.

#### 템플릿 사용 시 유의사항

- 부모 템플릿에 {% raw %}`{% block %}`{% endraw %} 태그가 많아질수록 좋다.
- 부모 템플릿의 {% raw %}`{% block %}`{% endraw %} 안에 있는 내용을 그대로 사용하고 싶다면 자식 템플릿에서 `{{ block.super }}` 변수를 사용한다.
- 가독성을 높이기 위해 {% raw %}`{% endblock content %}`{% endraw %}처럼 블록명을 기입해도 된다.

### 폼

폼 객체는 렌더링 이후에 사용자가 데이터를 채우는 것이 보통이므로, 빈 객체를 렌더링하는 일이 자주 발생한다. 그래서 뷰 함수에서 폼 객체를 생성할 때는 데이터 없이 만들 것인지, 아니면 데이터를 채워서 만들 것인지 적절히 구분해서 코딩해야 한다.

- 데이터가 없는 폼은 언바운드 폼이라고 하며, 렌더링되어 사용자에게 보여질 때 비어 있거나 디폴트 값으로 채워진다.
- 데이터가 있는 폼은 바운드 폼이라고 하며, 제출된 데이터를 갖고 있어서 데이터의 유효성 검사를 하는 데 사용된다.

모든 폼 클래스는 `django.forms.Form`의 자식 클래스로 생성됩니다.

{% raw %}

```python
from django import forms

class NameForm(forms.Form):
    your_name = forms.CharField(label='Your name', max_length=100)
```

{% endraw %}

label 속성: `<label>` 엘리먼트로 렌더링됨.
max_length 속성: 1) 사용자가 100글자 이상 입력하는 것을 브라우저가 방지할 수 있도록 해 준다. 2) 장고가 브라우저로부터 폼 데이터를 받았을 때 데이터의 길이가 유효한지 검사하는 데 사용된다.

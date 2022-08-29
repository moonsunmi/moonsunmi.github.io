---
layout: post
title:  "점프투 장고 공부 3"
date:   2022-08-19 22:26:44 +0900
categories: jumptodjango
---





admin.py
```python
from django.contrib import admin
from .models import Question

class QuestionAdmin(admin.ModelAdmin):
   search_fields = ['subject']


admin.site.register(Question, QuestionAdmin)
```

`render` 함수는 파이썬 데이터를 템플릿에 적용하여 HTML로 반환하는 함수이다. 즉, 위에서 사용한 `render` 함수는 질문 목록으로 조회한 `question_list` 데이터를 pybo/question_list.html 파일에 적용하여 HTML을 생성한후 리턴한다. 템플릿 파일은 HTML 파일과 비슷하지만 파이썬 데이터를 읽어서 사용할수 있는 HTML 파일이다. 


### 템플릿 위치

하나의 웹 사이트에서 여러 앱을 사용할 때 여러 앱의 화면을 구성하는 템플릿은 한 디렉터리에 모아 관리하는 편이 여러모로 좋다.

박응용 저자는 다음 구조를 선호한다.
* 모든 앱이 공통으로 사용할 템플릿 디렉터리 - projects/mysite/templates
* pybo 앱이 사용할 템플릿 디렉터리 - projects/mysite/templates/pybo
* common 앱이 사용할 템플릿 디렉터리 - projects/mysite/templates/common

question_list.html
{% raw %}
```html
{% if question_list %}
   <ul>
   {% for question in question_list %}
       <li><a href="pybo/{{ question.id }}">{{ question.subject }}</a></li>
   {% endfor %}
   </ul>
{% else %}
   <p>질문이 없습니다.</p>
{% endif %}
```
{% endraw %}


이렇게 없는 데이터를 요청할 경우 500 오류 페이지 보다는 "Not Found (404)" 페이지를 리턴하는 것이 바람직하다.

`Question.objects.get(id=question_id)`를 `get_object_or_404(Question, pk=question_id)`로 바꾸었다.

URL 링크의 구조가 자주 변경되는 경우를 대비해 URL에 대한 실제 링크 대신 링크의 주소가 1:1 매핑되어 있는 별칭을 사용
 
서로 다른 앱에서 동일한 URL 별칭을 사용하면 중복이 발생할 것이다. 이를 방지하려면 네임스페이스를 의미하는 `app_name` 변수를 지정해야한다. 

{% raw %}
```html
{% if question_list %}
   <ul>
   {% for question in question_list %}
       <li><a href="{% url 'pybo:detail' question.id %}">{{ question.subject }}</a></li>
   {% endfor %}
   </ul>
{% else %}
   <p>질문이 없습니다.</p>
{% endif %}
```
{% endraw %}


form 태그 바로 밑에 csrf_token 태그를 항상 위치시켜야 한다. POST 요청시 form 태그에 `csrf_token`이 없으면 장고는 오류를 낸다.


`<link>` 태그의 `rel` 속성은 현재 문서와 외부 리소스 사이의 연관 관계를 명시합니다. `rel` 속성은 `<link>` 요소에 반드시 명시되어야 하는 필수 속성입니다.


style.css
```html
textarea {
   width:100%;
}

input[type=submit] {
   margin-top:10px;
}
```


> css가 안 먹는 에러가 발생했었는데, setting 파일에서 DIRS를 DIR로 써서 그랬음. ‘-s’, ‘점과 쉼표' 같은 오타를 조심해야겠다.

```html
STATIC_URL = '/static/'
STATICFILES_DIRS = [
    BASE_DIR / 'static',
]
```


base.html
{% raw %}
```html
{% load static %}
<!doctype html>
<html lang="ko">
<head>
   <!-- Required meta tags -->
   <meta charset="utf-8">
   <meta name="viewport" content="width=devide-width, initial-scale=1, shrink-to-fit=no">
   <!-- Bootstrap CSS -->
   <link rel="stylesheet" type="text/css" href="{% static 'bootstrap.min.css' %}">
   <!-- pybo CSS -->
   <link rel="stylesheet" type="text/css" href="{% static 'style.css' %}">
   <title>Hello, Pybo!</title>
</head>
<body>
<!-- 기본 템플릿 안에 삽입될 내용 start! -->
{% block content %}
{% endblock %}
<!-- 기본 템플릿 안에 삽입될 내용 end! -->
</body>
</html>
```
{% endraw %}


question_list.html
{% raw %}
```html
{% extends 'base.html' %}
{% block content %}

<div class="container my3">
   <table class="table">
       <thead>
           <tr class="table-dark">
               <th>번호</th>
               <th>제목</th>
               <th>작성 일시</th>
           </tr>
       </thead>
       <tbody>
       {% if question_list %}
       {% for question in question_list %}
           <tr>
               <td>{{ forloop.counter }}</td>
               <td>
                   <a href="{% url 'pybo:detail' question.id %}">{{ question.subject }}</a>
               </td>
               <td>{{ question.create_date }}</td>
           </tr>
       {% endfor %}
       {% else %}
       <tr>
           <td colspan="3">질문이 없습니다.</td>
       </tr>
       {% endif %}
       </tbody>
   </table>
   <a href="{% url 'pybo:question_create' %}" class="btn btn-primary">질문 등록하기</a>
</div>
{% endblock %}
```
{% endraw %}




question_detail.html
{% raw %}
```html
{% extends 'base.html' %}
{% block content %}

<div class="container my-3">
   <!--질문-->
   <h2 class="border-bottom py-2">{{ question.subject }}</h2>
   <div class="card my-3">
       <div class="card-body">
           <div class="card-text" style="white-space: pre-line">{{ question.content }}</div>
           <div class="d-flex justify-content-end">
               <div class="badge bg-light text-dark p-2">
                   {{ question.create_date }}
               </div>
           </div>
       </div>
   </div>
   <!--답변-->
   <h5 class="border-bottom py-2">{{ question.answer_set.count }}개의 답변이 있습니다.</h5>
   {% for answer in question.answer_set.all %}
   <div class="card my-3">
       <div class="card-body">
           <div class="card-text" style="white-space: pre-line;">{{ answer.content }}</div>
           <div class="d-flex justify-content-end">
               <div class="badge bg-light text-dark p-2">
                   {{ answer.create_date }}
               </div>
           </div>
       </div>
   </div>
   {% endfor %}
   <!-- 답변 등록 -->
   <form action="{% url 'pybo:answer_create' question.id %}" method="post" class="my-3">
       {% csrf_token %}
       <div class="mb-3">
           <label for="content" class="form-label">답변 내용</label>
           <textarea name="content" id="content" class="form-control" rows="10"></textarea>
       </div>
       <input type="submit" value="답변등록" class="btn btn-primary">
   </form>
</div>
{% endblock %}
```
{% endraw %}


{% raw %}
base.html 템플릿을 상속하기 위해 {% extends 'base.html' %} 
{% block content %} 와 {% endblock %} 사이에 question_list.html에서만 쓰이는 내용을 작성
{% endraw %}



#### 장고에서의 폼(form)?
폼은 쉽게 말해 페이지 요청시 전달되는 파라미터들을 쉽게 관리하기 위해 사용하는 클래스이다. 

폼은 필수 파라미터의 값이 누락되지 않았는지, 파라미터의 형식은 적절한지 등을 검증할 목적으로 사용한다.

이 외에도 HTML을 자동으로 생성하거나 폼에 연결된 모델을 이용하여 데이터를 저장하는 기능도 있다.



forms.py
```python
from django import forms
from .models import Question


class QuestionForm(forms.ModelForm):
   class Meta:
       model = Question
       fields = ['subject', 'content']
```


장고의 폼은 일반 폼(`forms.Form`)과 모델 폼(`forms.ModelForm`)이 있는데 모델 폼은 모델(`Model`)과 연결된 폼으로 폼을 저장하면 연결된 모델의 데이터를 저장할수 있는 폼이다.



question_form.html
{% raw %}
```html
{% extends 'base.hmtl' %}
{% block content %}
<div class="container">
 <h5 class="my-3 border-bottom pb-2">질문 등록</h5>
 <form method="post">
   {% crsf_token %}
   {{ form.as_p }}
   <button type="submit" class="btn btn-primary">저장하</button>
 </form>
</div>
{% endblock %}
```
{% endraw %}

`{{ form.as_p }}`의 `form`은 `question_create` 함수에서 전달한 `QuestionForm`의 객체이다. 

`{{ form.as_p }}`는 폼에 정의한 `subject`, `content` 속성에 해당하는 HTML 코드를 자동으로 생성한다.


보통 `form` 태그에는 항상 `action` 속성을 지정하여 `submit` 실행시 `action`에 정의된 `URL`로 폼을 전송해야 한다.

`form` 태그에 `action` 속성을 지정하지 않으면 현재 페이지의 URL이 디폴트 `action`으로 설정된다.


view.py

```python

… (중략) ...
def question_create(request):
   if request.method == 'POST':  # '질문 등록' 화면에서 [저장하기] 버튼을 누른 경우 -> 질문 내용을 데이터베이스에 저장해야 함.
       form = QuestionForm(request.POST)
       # request.POST를 인수로 QuestionForm을 생성할 경우에는
       # request.POST에 담긴 subject, content 값이 QuestionForm의 subject, content 속성에 자동으로 저장되어 객체가 생성된다.
       if form.is_valid():
           question = form.save(commit=False)  # 임시저장
           question.create_date = timezone.now()
           question.save()
           return redirect('pybo:index')
   else:  # '질문 목록' 화면에서 [질문 등록하기] 버튼을 누른 경우
       form = QuestionForm()
   context = {'form':form}
   return render(request, 'pybo/question_form.html', context)
```



question_form.html
{% raw %}
```html
{% extends 'base.html' %}
{% block content %}
<div class="container">
 <h5 class="my-3 border-bottom pb-2">질문 등록</h5>
 <form method="post">
   {% csrf_token %}
   <!-- 오류 표시 start -->
   {% if form.errors %}
   <div class="alert alert-danger" role = "alert">
     {% for field in form %}
     {% if form.errors %}
     <div>
       <strong>{{ field.label }}</strong>
       {{ field.errors }}
     </div>
     {% endif %}
     {% endfor %}
   </div>
   {% endif %}
   <!-- 오류 표시 end -->
   <div class="mb-3">
     <label for="subject" class="form-label">제목</label>
     <input type="text" class="form-control" name="subject" id="subject" value="{{ form.subject.value|default_if_none:'' }}">
   </div>
   <div class="mb-3">
     <label for="content" class="form-label">내용</label>
     <textarea class="form-control" name="content" id="content" rows="10">{{ form.content.value|default_if_none:'' }}</textarea>
   </div>
   <button type="submit" class="btn btn-primary">저장하기</button>
 </form>
</div>
{% endblock %}
```
{% endraw %}







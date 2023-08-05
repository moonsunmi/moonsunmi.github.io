---
layout: post
title: "파이썬 웹 프로그래밍 공부 7"
date: 2022-09-04 22:18:12 +0900
tags: python-web-programming
---

각각의 폼 필드는 위젯 클래스를 가지고 있다. CharField 필드 타입은 TextInput이 디폴트 위젯이다. 이 위젯 클래스는 `<input type="text">`와 같은 HTML 폼 위젯으로 대응된다. 디폴트 위젯을 `<textarea>`로 변경하려면 폼 필드를 정의할 때 명시적으로 지정하면 된다.

```python

class NameForm(forms.Form):
    your_name = forms.CharField(label='Your name', max_length=100, widget=forms.Textarea)

NameForm: 폼 클래스
your_name: 폼 필드
CharField: 이 필드의 타입.
widget=form.Textarea: 디폴트 위젯인 TextInput 대신 Textarea로 변경한다.
```

`form.is_valid()` 메서드가 True이면 유효한 폼 데이터가 `cleaned_data` 속성에 담긴다. 이 데이터를 이용하여 데이터베이스를 변경하거나 로직에 따른 처리를 한다.

## 클래스형 뷰

장고의 URL 해석기는 요청과 관련된 파라미터들을 클래스가 아니라 함수에 전달하기 때문에, 클래스형 뷰는 클래스로 진입하기 위한 `as_view()` 클래스 메서드를 제공합니다. 이를 진입 메서드라고 한다.

함수형 뷰
{% raw %}

```python
from django.http import HttpResponse

def my_view(request):
    if request=='POST':
        # 뷰 로직 작성
        return HttpResponse('result')
```

{% endraw %}

클래스형 뷰

{% raw %}

```python
from django.http import HttpResponse
from django.views.generic import View

class MyView(View):
    def get(self, request):
        # 뷰 로직 작성
        return HttpResponse('result')
```

{% endraw %}

GET 요청이 들어 오면, 클래스형 뷰의 내부에 있는 dispatch() 메서드가 get() 메서드로 요청을 중계해 준다.

템플릿뷰 속성
url.py

```python
urlpatterns = [
    path('about/', AboutView.as_view())
]
```

views.py

```python
from django.views.generic import TemplateView
class AboutView(TemplateView):
    template_name = "about.html"
```

### 제네릭 뷰

우리가 작성하는 클래스형 뷰의 대부분은 장고가 제공하는 제네릭 뷰를 상속 받게 한다. 장고에서는 공통된 로직을 미리 개발해 놓고 제공하는 뷰를 제네릭 뷰라고 한다. 제네릭 뷰는 클래스형 뷰로 구성되어 있다.

- Base View: 뷰 클래스를 생성하고, 다른 제네릭 뷰의 부모 클래스를 제공하는 기본 제네릭 뷰
- Generic Display View: 객체의 리스트를 보여주거나, 특정 객체의 상세 정보를 보여줍니다.
- Generic Edit View: 폼을 통해 객체를 생성, 수정, 삭제하는 기능을 제공
- Generic Date View: 날짜 기반 객체의 연/월/일 페이지로 구분해서 보여준다.

함수형 뷰

```python

from django.http import HttpResponseRedirect
from django.shortcuts import render

from .forms import MyForm

def myview(request):
    if request.method == "POST":
        form = MyForm(request.POST)
        if form.is_valid():
            # cleaned_data로 관련 로직 처리
            return HttpResponseRedirect('/success/')
    else:
        form = MyForm(initial={'key': 'value'})

    return render(request, 'form_template.html', {'form': form})
```

클래스형 뷰
{% raw %}

```python
from django.http import HttpResponseRedirect
from django.shortcuts import render
from django.views.generic import View

from .forms import MyForm

class MyFormView(View):
    form_class = MyForm
    initial = {'key': 'value'}
    template_name = 'form_template.html'

    def get(self, request, *args, **kwargs):  # 최초의 GET
        form = self.form_class(initial=self.initial)
        return render(request, self.template_name, {'form': form})

    def post(self, request, *args, **kwargs):
        form = self.form_class(request.POST)
        if form.is_valid():
            # cleaned_data로 관련 로직 처리
            return HttpResponseRedirect('/success/')    # 유효한 데이터를 가진 POST

        return render(request, self.template_name, {'form': form})  # 유효하지 않은 데이터를 가진 POST

```

{% endraw %}

FormView 제네릭 뷰

```python
from .form import MyForm
from django.views.generic.edit import FormView

class MyFormView(FormView):
    form_class = MyForm
    template_name = 'form_template.html'
    success_url = '/success/'

   def form_valid(self, form):
        # cleaned_data 관련 로직 처리
        return super(MyFormView, self).form_valid(form)
```

FormView 제네릭 뷰는 get(), post() 메서드도 이미 정의되어 있음.

- form_valid() 함수: 유효한 폼 데이터로 처리할 로직 코딩. super()함수를 사용하면, success_url로 리다이렉션 처리된다.

### 로그

- 로거: 로깅 시스템의 시작점으로, 로그 메시지를 처리하기 위해 메시지를 담아두는 저장소라고 할 수 있다.
- 핸들러: 로거에 있는 메시지에 무슨 작업을 할지 결정하는 엔진
- 필터: 로그 레코드가 로거에서 핸들러로 넘겨질 때, 필터를 사용해서 로그 레코드에 추가적인 제어를 할 수 있다.
- 포매터: 로그 레코드가 최종적으로 텍스트로 표현된다. 이때 사용할 포맷을 지정한다.

## 5장 실습 예제 확장하기

books/models.py
{% raw %}

```python
from django.db import models


class Book(models.Model):
    title = models.CharField(max_length=100)
    authors = models.ManyToManyField('Author')
    publisher = models.ForeignKey('Publisher', on_delete=models.CASCADE)
    publication_date = models.DateField()

    def __str__(self):
        return self.title


class Author(models.Model):
    salutation = models.CharField(max_length=100)
    name = models.CharField(max_length=50)
    email = models.EmailField()

    def __str__(self):
        return self.name


class Publisher(models.Model):
    name = models.CharField(max_length=50)
    address = models.CharField(max_length=200)
    website = models.URLField()

    def __str__(self):
        return self.name

```

{% endraw %}

ForeignKey 필드를 사용할 때는 on_delete 옵션을 필수로 지정해야 한다.

views.py

{% raw %}

```python
from django.views.generic.base import TemplateView
from django.views.generic import ListView
from django.views.generic import DetailView
from books.models import Book, Author, Publisher


class BooksModelView(TemplateView):
    template_name = 'books/index.html'

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['model_list'] = ['Book', 'Author', 'Publisher']
        return context


#--- ListView
class BookList(ListView):
    model = Book


class AuthorList(ListView):
    model = Author


class PublisherList(ListView):
    model = Publisher


#--- DetailView
class BookDetail(DetailView):
    model = Book


class AuthorDetail(DetailView):
    model = Author


class PublisherDetail(DetailView):
    model = Publisher
```

{% endraw %}

get_context_data() 메소드를 정의할 때는 반드시 첫 줄에 super() 메서드를 호출해야 한다.

BookList: Book 테이블로부터 모든 레코드를 가져와 object_list라는 컨텍스트 변수를 구성한다. 템플릿 파일은 디폴트로 books/book_list.html 파일(앱이름/모델이름.html)이 된다.

BookDetail: Book 테이블로부터 모든 레코드를 가져와 object라는 컨텍스트 변수를 구성한다. 템플릿 파일은 디폴트로 books/book_detail.html 파일(앱이름/모델이름.html)이 된다.

장고에서 권장하는 3단계 템플릿 상속 구조:
base.html <-- base_books.html <-- xxx_detail.html

xxx_list.html
{% raw %}

```html
{% extends "base_books.html" %} {% block content %}
<h2>Book List</h2>
<ul>
  {% for book in object_list %}
  <li><a href="{% url 'books:book_detail' book.id %}">{{ book.title }}</a></li>
  {% endfor %}
</ul>
{% endblock content %}
```

{% endraw %}

xxx_detail.html
{% raw %}

```html
{% extends "base_books.html" %} {% block content %}
<h1>{{ object.title }}</h1>
<br />
<li>Authors:</li>
{% for author in object.authors.all %} {{ author }} {% if not forloop.last %},{%
else %}{% endif %} {% endfor %}
<li>publisher: {{ object.publisher }}</li>
<li>publication date: {{ object.publication_date }}</li>
{% endblock content %}
```

{% endraw %}

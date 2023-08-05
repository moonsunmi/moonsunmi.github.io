---
layout: post
title: "파이썬 웹 프로그래밍 공부 3"
date: 2022-08-15 22:25:12 +0900
tags: python-web-programming
---

시점에 따라 응답 내용(예: 현재 시간)이 달라지는 동적요청은 웹 서버가 아니라 별도의 애플리케이션에서 작성하는 게 보통임. 그래서 웹 서버는 동적요청을 애플리케이션으로 넘겨주고 그 결과를 받는 기능이 필요하다. 이때 데이터를 주고 받기 위한 규격

- WSGI: 웹 서버와 웹 애플리케이션을 연결해주는 규격. 전통적인 웹 CGI 규격 보완. wsgiref 모듈

WSGI 규격만 맞추면 어떤 웹 서버(아파치, Nginx)에서도 파이썬 애플리케이션을 실행할 수 있다.
이때 아파치, Nginx는 웹 서버와 파이썬 웹 애플리케이션 중간에서 WSGI 통신 규격을 처리해줘야 하는데, mod_wsgi, uWSGi, Gunicorn 같은 WSGI 서버.

주의사항:

1. 개발이 필요한 애플리케이션을 함수 또는 클래스의 메소드로 정의하고, 애플리케이션 함수의 인자는 다음과 같이 정의한다.

`def application_name(environ, start_response):`

`start_response`: 응답 위해 반드시 호출해야.

2. `start_response` 함수 인자 `start_response(status, headers)`

3. 애플리케이션 함수의 리턴값은 리스트나 제너레이터와 같은 iterable 타입이어야 한다.

#### wsgiref.simple_server 모듈

WSGI 웹 서버에 대한 참조 서버. WSGIServer 클래스와 WSGIRequestHandler 클래스를 정의하고 있다.

```python

from wsgiref.simple_server import make_server

def my_app(environ, start_response):

   status = '200 OK'
   headers = [('Content-Type', 'text/plain')]
   start_response(status, headers)


   response = [b'This is a sample WSGI Application.']

   return response


if __name__ == '__main__':
   print('Started WSGI Server on port 8888...')
   server = make_server('', 8888, my_app)
   server.serve_forever()
```

## 3장

장고는 기본적으로 MVC 패턴에 해당하는 MVT 패턴에 따라 개발하도록 설계되었다.

- 웹 애플리케이션: 웹 사이트의 전체 프로그램 또는 모듈화된 단위 프로그램

##### 애플리케이션 개념을 웹 서버 개발측면에서 좀 더 구체화한 용어들

- 프로젝트: 웹 사이트에 대한 전체 프로그램
- 애플리케이션: 모듈화된 단위 프로그램

즉, 애플리케이션 프로그램들이 모여서 프로젝트를 개발하는 개념

##### MVC 패턴

데이터(Model), 사용자 인터페이스(View), 데이터를 처리하는 로직(Controller)을 구분해서 한 요소가 다른 요소들에 영향을 주지 않도록 설계하는 방식입니다.

#####장고 프레임워크에서는 MVT 패턴.

- Model: 데이터베이스에 저장되는 데이터(블로그 내용을 데이터베이스로부터 가지고 오거나 저장, 수정하는 기능)
- Template: 사용자에게 보여주는 UI 부분(화면 출력을 위해 디자인과 테마를 적용해서 보여주는 페이지를 만들어 주는 역할)
- View: 실질적으로 프로그램 로직이 동작하여 데이터를 가져오고 적절하게 처리한 결과를 템플릿에 전달하는 역할을 수행한다.(버튼을 눌렀을 때 어떤 함수를 호출하며 데이터를 어떻게 가공할지 결정하는 역할)

##### 장고에서 MVT 패턴에 따라 처리하는 과정:

1. 클라이언트로부터 요청을 받으면 URLconf를 이용해 URL 분석
2. URL 분석 결과를 통해 해당 URL에 대한 처리를 담당할 뷰를 결정한다.
3. 뷰는 자신의 로직을 실행. 데이터베이스 처리가 필요하면 모델을 통해서 처리한다. 로직이 끝나면 템플릿을 사용해 HTML 파일을 생성한다. 그다음 클라이언트에 보냄.

##### 모델이란

사용될 데이터에 대한 정의를 담고 있는 장고의 클래스입니다. 장고는 ORM 기법을 사용하여 애플리케이션에서 사용할 데이터베이스를 클래스로 매핑해서 코딩할 수 있다. 하나의 모델 클래스는 하나의 테이블에 매핑. 모델 클래스의 속성은 테이블의 컬럼에 매핑

- ORM: Object-Relational Mapping. 자동으로 적절한 SQL 구문이나 데이터베이스 API를 호출하여 객체를 사용해 데이터를 처리할 수 있게 해 줌.

#### URLconf - URL 정의

클라이언트로부터 요청을 받으면, URL 패턴이 매칭되는지부터 분석한다. URL을 정의하기 위해서 URL과 처리함수를 별도로 정의하여 매핑한다.

##### URL 분석하는 순서

setting.py 파일의 ROOT_URLCONF 항목을 읽어 최상위 URLconf(urls.py)의 위치를 알아낸다.
URLconf를 로딩해서 urlpatterns 변수에 지정되어 있는 URL 리스트를 패턴이 매치되는지 검사
매치된 URL의 뷰를 호출한다. 호출 시 HttpRequest 객체와 매칭할 때 추출된 단어를 뷰에 인자로 넘겨준다.

`path(‘articles/<int:year>/’, views.year_archive)
-> /articles/2018/이면 뷰 함수를 views.years_archive(request, year=2018)`처럼 호출한다.

- slug: ASCII, 숫자, 하이픈, 밑줄로만 구성됨
- uuid: uuid는 네트워크상에서 중복되지 않는 고유한 식별자인 UUID를 생성할 때 사용하는 모듈이다. UUID(Universally Unique IDentifier)는 네트워크상에서 고유성을 보장하는 ID를 만들기 위한 표준 규약이다. UUID는 다음과 같이 32개의 16진수로 구성되며 5개의 그룹으로 표시되고 각 그룹은 붙임표(-)로 구분한다.
- path: /를 포함한 모든 문자열과 매치. URL 전체를 추출하고자 할 때 많이 사용한다.

##### View - 로직 정의

1. URL 분석 후, 해당 URL에 매핑된 뷰를 호출한다.
2. view는 첫 번째 인자로 HttpRequest 객체를 받고, 처리 후 HttpResponse 객체를 반환한다.
3. 뷰는 별도로 작성된 템플릿 파일을 해석해서 HTML 코드를 생성하고(템플릿 파일에서 생성) 이를 HttpResponse 객체에 담아서 클라이언트에게 응답한다.

##### Template - 화면 UI 정의

1. 장고가 클라이언트에게 반환하는 최종 응답은 HTML 텍스트이다.
2. 클라이언트는 응답으로 받은 HTML 텍스트를 해석해서 웹 브라우저 화면에 UI를 보여주는 것.
3. 이런 과정에서 개발자가 작성하는 \*.html 파일을 템플릿이라 부른다. 템플릿 파일은 적절한 디렉토리에 있어야 한다.

##### MVT 코딩 순서

보통은 독립적으로 개발할 수 있는 모델을 먼저 코딩하고, 뷰와 템플릿은 서로 영향을 미치므로 모델 이후에 같이 코딩함.

#### 애플리케이션 설계하기

프로젝트란 개발 대상이 되는 전체 프로그램. 프로젝트를 몇 개의 기능 그룹으로 나누었을 때, 프로젝트 하위의 서브 프로그램을 애플리케이션이라 한다.

- 장고는 ForeignKey인 경우 자동으로 Index를 생성한다.

ForeignKey? 외래키는 두 테이블을 서로 연결하는 데 사용되는 키이다. 외래키가 포함된 테이블을 자식 테이블이라고 하고 외래키 값을 제공하는 테이블을 부모 테이블이라한다.

#### 프로젝트 뼈대 만들기

프로젝트에 필요한 디렉터리, 파일을 구성하고 설정 파일을 세팅.

settings.py: 프로젝트에 필요한 설정값 지정해 준다.

1. `ALLOWED_HOSTS` 항목을 적절하게 지정해야 한다.
   개발 모드이므로 `DEBUG=True`, `ALLOWED_HOSTS = []`이더라도, 127.0.0.1, localhost는 자동으로 됨.
2. 프로젝트에 포함되는 애플리케이션은 모두 설정 파일에 등록되어야 한다.
   `polls` 애플리케이션을 만들었으므로, `polls` 디렉터리에 자동 생성된 apps.py의 `PollsConfig`를 경로까지 지정해줌.
3. 프로젝트에 사용할 데이터베이스 엔진
   디폴트로 SQLite3 설정. 변경하고 싶다면 수정.
4. 타임존 지정. 한국 시간으로

##### 기본 테이블 생성

migrate는 데이터베이스에 변경사항이 있을 때 반영해 주는 명령. 장고는 프로젝트 개발 시점에 이 명령을 실행하여 데이터베이스 생성해둠(?)

runserver: 장고에서 제공하는 간단한 테스트용 웹 서버

##### 테이블 정의

`Foreign Key`로 지정된 칼럼은 `_id` 접미사가 붙는다.

models.py 파일에서 정의한 테이블을 Admin 사이트에서 보이게 하려면, admin.py 파일에 등록해 줘야 한다.

클래스로 테이블 변경한 것을, 실제 데이터베이스에 반영해야 한다. migrate 명령으로.

#### URLconf 코딩

url 패턴 매칭은 위에서 아래로 진행하므로 정의하는 순서에 유의해야 합니다.

urlpatterns = [
path('admin/', admin.site.urls),
path('polls/', views.index, name='index'),
path('polls/<int:question_id>/', views.detail, name='detail'),
path('polls/<int:question_id>/results/', views.results, name='results'),
path('polls/<int:question_id>/vote/', views.vote, name='vote'),
]

`path`의 `name` 인자: URL 패턴에 붙인 이름. 템플릿 파일에서 주로 사용한다.

`URLconf`를 코딩할 때, mysite/urls.py와 polls/urls.py처럼 2개의 파일에 작성하는 게 더 좋다.

mysite/urls.py

```python
from django.contrib import admin
from django.urls import path

urlpatterns = [
   path('admin/', admin.site.urls),
   path('polls/', include('polls.urls')),
]
```

polls/urls.py

```python
from django.urls import path
from . import views

app_name = 'polls'
urlpatterns = [
   path('', views.index, name='index'),
   path('<int:question_id>/', views.detail, name='detail'),
   path('<int:question_id>/results/', views.results, name='results'),
   path('<int:question_id>/vote', views.vote, name='vote'),
]
```

여기에서 `app_name`은 이름공간 역할

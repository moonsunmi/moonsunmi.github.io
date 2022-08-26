---
layout: post
title:  "점프투 장고"
date:   2022-08-17 22:18:24 +0900
categories: jumptodjango
---


## 1장

#### 가상환경
파이썬 가상 환경을 이용하면 하나의 PC 안에 독립된 가상 환경을 여러 개 만들 수 있다. 하나의 PC에 서로 다른 버전의 파이썬과 라이브러리를 쉽게 설치해 사용할 수 있다. 

`C:\venvs> python -m venv mysite`


`python -m venv`는 파이썬 모듈 중 `venv`라는 모듈을 사용한다는 의미. 

가상 환경을 사용하려면 가상 환경에 진입해야 한다. 가상 환경에 진입하려면 mysite 가상 환경에 있는 Scripts 디렉터리의 activate 명령을 수행해야 한다. 
 
#### 가상 환경에 진입한 상태에서 장고 설치
`(mysite) C:\venvs\mysite\Scripts>`

프로젝트 안에는 여러 개의 앱이 존재한다. 이 앱들이 모여 웹 사이트를 구성한다.

`(mysite) C:\projects\mysite>django-admin startproject config .`

 현재 디렉터리인 mysite를 기준으로 프로젝트를 생성하겠다는 의미이다.

파이참 설치 후 프로젝트를 열고, 파이썬 인터프리터 위치를 가상 환경 위치로 수정한다.


#### App
프로젝트 단독으로는 아무런 일도 할 수 없다. 프로젝트에 기능을 추가하기 위해서는 앱을 생성해야 한다. 

장고의 urls.py 파일은 페이지 요청이 발생하면 가장 먼저 호출되는 파일로 URL과 뷰 함수 간의 매핑을 정의한다. 

````python
from django.contrib import admin
from django.urls import path

from pybo import views

urlpatterns = [
   path('admin/', admin.site.urls),
   path('pybo/', views.index)
]
````
`views.index`는 views.py 파일의 `index` 함수를 의미한다.



````python
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.

def index(request):
   return HttpResponse("안녕하세요.")
````

`HttpResponse`는 요청에 대한 응답을 할 때 사용한다. `request`는 HTTP 요청 객체이다. 





---
layout: post
title: "점프투 장고 공부 2"
date: 2022-08-18 22:13:14 +0900
tags: jumptodjango
---

config의 urls.py 파일은 앱이 아닌 프로젝트 성격의 파일이므로 이곳에는 프로젝트 성격의 URL 매핑만 추가되어야 한다. 따라서 pybo 앱에서만 사용하는 URL 매핑을 config/urls.py 파일에 계속 추가하는 것은 좋은 방법이 아니다.

config/urls.py

```python

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
   path('admin/', admin.site.urls),
   path('pybo/', include('pybo.urls')),
]
```

pybo/urls.py

```python
from django.urls import path

from . import views

urlpatterns = [
   path('', views.index),
]
```

`admin`, `auth`, `contenttypes`, `sessions` 앱들은 데이터베이스 필요하다. 이들은 migrate해야 한다.migrate를 수행하면 admin, auth, contenttypes, sessions 앱들이 사용하는 테이블들이 생성된다.

장고의 ORM(Object Relational Mapping)을 사용하면 쿼리문을 몰라도 데이터 작업을 쉽게 할 수 있다.
쿼리문이란 데이터베이스의 데이터를 생성, 조회, 수정, 삭제하기 위해 사용하는 질의문(문법)이다.
`CASCADE` 옵션은 질문을 삭제하면 그에 달린 답변들도 모두 함께 삭제한다.

models.py

```python
from django.db import models


class Question(models):
   subject = models.CharField(max=200)
   content = models.TextField()
   create_date = models.DateTimeField()


class Answer(models):
   answer = models.ForeignKey(Question, on_delete=models.CASCADE())
   content = models.TextField()
   create_date = models.DateTimeField()
```

모델이 신규로 생성되거나 변경되면 makemigrations 명령을 먼저 수행한 후에 migrate 명령을 수행해야. pybo\migrations\0001_initial.py 라는 파이썬 파일이 자동으로 생성된다

데이터가 1건 생성되면 반드시 다음처럼 id 값이 생성된다. id는 모델 데이터의 유일한 값으로 프라이머리 키(PK:Primary Key)라고도 한다. 이 id 값은 데이터를 생성할 때마다 1씩 증가된다.

Question 모델에는 answer_set 이라는 속성이 없지만 Answer 모델에 Question 모델이 ForignKey로 연결되어 있기 때문에 `q.answer_set`과 같은 역방향 접근이 가능하다.

---
layout: post
title:  "파이썬 웹 프로그래밍 공부 9"
date:   2022-09-06 22:11:17 +0900
categories: python_web_programming
---


## 6장 Django의 웹 서버 연동 원리

wsgi.py 파일에는 application 객체가 정의되어 있다.

장고는 웹 애플리케이션을 만들어주는 프레임워크고, WSGI 규격의 애플리케이션 스펙을 구혆기 위해 wsgi.py 파일을 제공한다. 규격에 따라 WSGI 서버에서는 application 호출자를 호출하는데, wsgi.py 파일에서 이 호출자를 정의한다. 장고에서는 이 호출자를 WSGIHandler 클래스로 정의한다.


WSGI 규격: 파이썬 언어로 애플리케이션을 좀 더 쉽게 작성할 수 있도록 웹 서버와 웹 애플리케이션 간에 연동 규격을 정의한 것
ex) 개발이 필요한 애플리케이션을 함수 또는 클래스의 메소드로 정의하고, 애플리케이션 함수의 인자는 다음과 같이 정의한다.
def application_name(environ, start_response):


파이썬에서는 WSGI 규격을 맞추면 어떤 웹서버(Apache, Nginx 등)에서도 파이썬 애플리케이션을 실행할 수 있다.

그러나 Apache, Nginx 등은 일반 범용 웹 서버로, WSGI 처리 기능이 없다. 이런 웹 서버와 파이썬 웹 애플리케이션 중간에서 WSGI 통신 규격을 처리해 주는 것이 mod_wsgi, uWSGI, Gunicorn 같은 WSGI 서버이다.


DEBUG = False로 되어 있으면, 반드시 settings 모듈의 ALLOWED_HOSTS 항목을 설정해야 한다.





---
layout: post
title:  "점프 투 장고 공부 13"
date:   2022-09-01 22:35:11 +0900
categories: jumptodjango
---

### 설정 자동화

DJANGO_SETTINGS_MODULE 환경 변수는 장고 서버 실행 시 사용하는 `--settings=config.settings.local` 옵션을 대신하는 환경변수


다음과 같은 에러가 난다.

`CommandError: You must set settings.ALLOWED_HOSTS if DEBUG is False.`

이 오류를 잡기 위해 시간을 많이 썼는데, 재실행을 하면 되는 거였다...
안 되면 재실행부터 시도해 보는 걸로...



### 서버 설정 자동화

> nano editor 맥에서 저장 & 종료하려면
> To save the file, press Control-O.
> At the filename prompt, press Enter.
> To exit, press Control-X.


SSH 터미널 접속

`ssh -i ~/mysite.pem ubuntu@43.200.117.214`



* 정적 파일 요청
css, js, jpg, png와 같은 정적 파일 요청

http://43.200.117.214:8000/static/bootstrap.min.css

* 동적 파일 요청
응답이 수시로 변하는 요청을 동적 페이지 요청



웹 서버는 웹 브라우저의 정적 요청과 동적 요청을 처리


웹 서버에 동적 페이지 요청이 들어오면 웹 서버는 파이썬 프로그램을 호출해야
파이썬 프로그램을 호출하는 WSGI(web server gateway interface) 서버 - 구니콘

웹서버에 동적 페이지 요청이 발생하면 웹 서버는 WSGI 서버를 호출하고 WSGI 서버는 다시 WSGI 애플리케이션(장고)을 호출한다. 


서버환경에 Gunicorn을 설치

구니콘에서는 포트보다는 소켓 방식이 더 낫다.

`gunicorn --bind unix:/tmp/gunicorn.sock config.wsgi:application`

유닉스 소켓 방식은 Nginx 같은 웹서버가 반드시 필요하다.



### AWS에 gunicorn 등록하기



서비스 파일(/etc/systemd/system/mysite.service)
```
[Unit]
Description=gunicorn daemon
After=network.target

[Service]
User=ubuntu
Group=ubuntu
WorkingDirectory=/home/ubuntu/projects/mysite
EnvironmentFile=/home/ubuntu/venvs/mysite.env
ExecStart=/home/ubuntu/venvs/mysite/bin/gunicorn \
        --workers 2 \
        --bind unix:/tmp/gunicorn.sock \
        config.wsgi:application

[Install]
WantedBy=multi-user.target
```


## Nginx 설치

웹서버(Web Server)는 브라우저의 정적 페이지 요청을 처리하고 동적 페이지 요청인 경우 WSGI 서버를 호출하는 역할을 한다.


/etc/nginx/sites-available/mysite
```
server {
        listen 80;
        server_name 3.37.58.70;

        location = /favicon.ico { access_log off; log_not_found off; }

        location /static {
                alias /home/ubuntu/projects/mysite/static;
        }

        location / {
                include proxy_params;
                proxy_pass http://unix:/tmp/gunicorn.sock;
        }
}
```


### 서버 관리자
서버용 관리자 계정을 만든다.
Nginx는 범용 웹서버기 때문에 static file의 위치를 잡아 줘야 한다.

projects\mysite\config\settings\prod.py

```python
from .base import *

ALLOWED_HOSTS = ['43.200.117.214']
STATIC_ROOT = BASE_DIR / 'static/'
STATICFILES_DIRS = []

```


수정사항 반영 후에는 구니콘을 재실행해야 한다.

`sudo systemctl restart gunicorn.service`



### DEBUG

config/urls.py
```python
from django.contrib import admin
from django.urls import path, include
from pybo.views import base_views
import logging

logging.debug('Debug Message')

urlpatterns = [
    path('admin/', admin.site.urls),
    path('pybo/', include('pybo.urls')),
    path('common/', include('common.urls')),
    path('', base_views.index, name='index'),
]

handler404 = 'common.views.page_not_found'

```
404 오류가 발생하면 common/views.py 파일의 page_not_found 함수가 호출된다.


common/views.py
```python
def page_not_found(request, exception):
    return render(request, 'common/404.html', {})
```

page_not_found 함수는 `request` 외에 `exception` 매개변수를 하나 더 받는다.


### 로깅

보통 운영 환경에서는 오류 식별을 위해 로그 파일을 사용한다.


로그 레벨은 다음과 같이 5단계로 구성된다. 
1단계 DEBUG: 디버깅 목적으로 사용
2단계 INFO: 일반 정보를 출력할 목적으로 사용
3단계 WARNING: 경고 정보를 출력할 목적으로(작은 문제) 사용
4단계 ERROR: 오류 정보를 출력할 목적으로(큰 문제) 사용
5단계 CRITICAL: 아주 심각한 문제를 출력할 목적으로 사용


> 도메인 이후의 실습은 유료이기도 하고, 필요할 때 다시 보면 될 것 같아 실습하지 않았다.




## 점프투장고 리뷰

점프투 시리즈, 박응용 저자의 책답게 친절하게 써 있다. 설명이 조금 부족하다고 느낄 순 있지만 장고로 웹 프로그래밍을 처음 경험해 봐야 하는 사람에게는 막히지 않게 실습이 잘 진행되는 게 우선인 것 같다. 그 목적을 충분히 달성한 좋은 책이었다.





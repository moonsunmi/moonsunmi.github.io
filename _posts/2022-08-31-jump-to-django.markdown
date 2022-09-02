---
layout: post
title:  "점프 투 장고 공부 12"
date:   2022-08-31 22:25:15 +0900
categories: jumptodjango
---

서버는 보통 PC보다 너비가 더 크고 납작하며 비싸다. 서버를 운영하려면 데이터베이스 설치, 네임 서버 설치, 도메인 등록, 백업 등 해야 할 일이 정말 많다.

클라우드 시스템은 인터넷 서비스 형태로 서버를 관리할 수 있게 해준다. 클릭 몇 번으로 서버, 운영체제, 데이터베이스 등과 같은 서버를 운용하는 데 필요한 모든 것을 선택하여 설치할 수 있다.


AWS 계정을 생성하고, 월 5달러, 3개월 무료 신청한 후 우분투 서버를 만들었다.

내 퍼블릭 고정 ip 주소: 43.200.117.214



github에 올리고, 그 파일을 가져온다.

> 다음과 같은 에러가 난다면,
> Error: That port is already in use.
> 다음 명령어를 입력해 8000 포트와 관련된 모든 프로세스를 죽인다.
> sudo lsof -t -i tcp:8000 | xargs kill -9



### 서버용과 로컬용으로 settings.py 분리

settings.py 파일을 base.py로 바꾸고, settings 폴더 안에 base.py, local.py, prod.py를 넣는다.

local.py
```python
from .base import *

ALLOWED_HOSTS = []
```

prod.py
```python
from .base import *

ALLOWED_HOSTS = ['43.200.117.214']
```

로컬환경에서 장고 서버를 동작할 때는 `--settings=config.settings.local` 옵션을 사용하고 서버환경에서 장고 서버를 동작할 때는 `--settings=config.settings.prod`를 사용해야 한다.



### 로컬 설정 자동화

로컬 설정 자동화를 했는데도 다음과 같은 에러가 난다.

`CommandError: You must set settings.ALLOWED_HOSTS if DEBUG is False.`


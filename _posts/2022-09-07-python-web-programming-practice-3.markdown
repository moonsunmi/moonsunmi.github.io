---
layout: post
title:  "파이썬 웹 프로그래밍 공부 실전편 3"
date:   2022-09-07 22:08:42 +0900
categories: python_web_programming_practice
---


마이그레이션: 테이블 및 필드의 생성, 삭제, 변경 등과 같은 데이터베이스에 대한 변경 사항을 알려주는 정보


URLconf
프로젝트 URL과 앱들의 URL로 나누어 정의하는 걸 추천한다.


프로젝트 템플릿 디렉터리는 TEMPLATES 설정의 DIRS 항목에 지정된 디렉터리다. base.html 등 전체 프로젝트의 룩앤필에 관련된 파일들을 모아두고, 각 앱에서 사용하는 템플릿 파일들은 앱 템플릿 디렉터리에 위치시킨다.


독립된 가상 환경이 필요한 이유
외부의 라이브러리들은 서로 의존성을 갖고 있는 경우가 많아 버전이 맞지 않을 경우 오작동을 일으키기도 한다. 별도의 개발 환경에 설치함으로써 다른 파이썬 프로그램에는 영향을 주지 않도록 하기 위한 것이다.


## 가상 환경 만들기

1. venvs 디렉터리에서 python -m venv vDjBook 명령어로 만듦.
2. vDjbook 가상환경에서 django 설치

Django 설치 시 다음과 같은 에러가 발생했다.

  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for backports.zoneinfo
Failed to build backports.zoneinfo
ERROR: Could not build wheels for backports.zoneinfo, which is required to install pyproject.toml-based projects











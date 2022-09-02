---
layout: post
title:  "파이썬 웹 프로그래밍 공부 5"
date:   2022-09-02 22:15:44 +0900
categories: python_web_programming
---


## 13장 Model

models.py 파일에는 테이블을 정의하는 것이 기본.
ORM 방식에서는 관련 변수 메소드를 추가적으로 정의할 수 있는 장점이 있다.

테이블의 컬럼(장고에서는 테이블의 필드 또는 모델 필드라고 함)은 모델 클래스의 속성으로 정의, 테이블과 관련된 함수는 모델 클래스의 메소드로 정의


#### 마이그레이션
데이터베이스의 테이블 및 필드에서 생긴 변경사항을 알려주는 정보이다.
makemigrations 명령으로 polls/migrations 디렉토리 하위에 마이그레이션 파일들이 생긴다.


#### import 문법
polls
ㄴ models.py
ㄴ admin.py

`from polls import models.Question` 형태를 추천한다.



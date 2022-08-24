---
layout: post
title:  "장고 걸스 공부 2"
date:   2022-08-12 22:21:35 +0900
categories: django-girls
---




블로그 글 모델 만들기부터..!

* 배포: 애플리케이션을 인터넷에 올려놓아 다른 사람들도 볼 수 있게 해주는 것



github과 연결할 때 다음과 같은 오류가 발생했다.
github에서 비밀번호 인증 지원을 더 이상하지 않는 것 같은데, 다음 사이트에서 확인하여 해결하였다.

https://hyeo-noo.tistory.com/184



장고는 "WSGI 프로토콜(WSGI protocol)"을 사용해 작동합니다. 


배포단계에서 오류 났다.
https://www.pythonanywhere.com/forums/topic/12443/

python 버전 문제일 수 있다는 답변. 내 파이썬 버전은 3.8.9이다. 실습에서는 3.6으로 나오는데, 그대로 따라 했음…. 

내 버전에 맞춰서 해야 하나? 생각이 들었지만, pythonanywhere 안에서만 맞춰주면 되는 건가 싶어서 3.6으로 함… 3.8로 수정해서 해결


* 템플릿이란, 서로 다른 정보를 일정한 형태로 표현하기 위해 재사용 가능한 파일을 말한다. 


=> my-first-blog 경로에 문제가 있었다. my-first-blog/my-first-blog였던 걸 수정함.





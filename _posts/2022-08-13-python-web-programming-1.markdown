---
layout: post
title:  "파이썬 웹 프로그래밍 공부 1"
date:   2022-08-13 22:12:45 +0900
categories: python_web_programming
---


## 1장 웹 프로그래밍의 이해

웹 프로그래밍이란? HTTP 프로토콜로 통신하는, 클라이언트와 서버를 개발하는 것.

리눅스 curl 명령은 HTTP, HTTPS, FTP 등 여러 프로토콜을 사용하여 데이터를 송수신할 수 있는 명령. 웹 클라이언트 역할 수행


> 리눅스 telnet을 사용하려 하는데, 오류가 발생했다.<br>
> zsh: command not found: telnet<br>
> macOS에서 telnet 사용하려면 install해야 한다.<br>
> https://osxdaily.com/2018/07/18/get-telnet-macos/

> homebrew 깔다가 에러가 났다.<br>
> error: Not a valid ref: refs/remotes/origin/master<br>
> 찾아보니 brew 설치 중 중단되어서 그런 거 같다.<br>
> https://worldseawater.tistory.com/87
> 오래 걸리길래 잘못되었나 싶어서 중간에 꺼 버렸다… ㅠ doctor가 하라는데로 다음의 명령어를 실행해서 homebrew를 깔았다.<br>
> `rm -rf "/opt/homebrew/Library/Taps/homebrew/homebrew-core"`<br>
> `brew tap homebrew/core`
> 명의다! 잘 된다.

brew install telnet으로 텔넷 설치까지 완료.

직접 만든 클라이언트로 요청할 수도 있다.
```python
import urllib.request
print(urllib.request.urlopen("http://www.example.com").read().decode('utf-8'))
```



* HTTP 프로토콜: 웹 서버와 웹 클라이언트 사이에서 데이터를 주고받기 위해 사용하는 통신 방식. TCP/IP 프로토콜 위에서 동작한다.


#####HTTP 메시지의 구조

스타트라인 (요청 라인 또는 상태라인)<br>
—----------<br>
헤더<br>
—---------<br>
빈 줄<br>
—--------<br>
바디<br>



스타트라인은 요청 메시지면 요청라인, 응답 메시지일 때 상태라인이라고 한다.


##### 요청 메시지의 예시
<pre>1   2                                                 3
GET http://www.example.com:8080/book/shakespeare HTTP/1.1
</pre>
1. 요청방식
2. 요청 URL
3. 프로토콜 버전



HTTP 메서드를 통해서 클라이언트가 원하는 처리 방식을 서버에 알려준다.
* GET : Read
* POST : Create(블로그에 글을 등록하는 경우처럼 리소스를 생성하는 기능.)
* PUT: Update
* DELETE: delete
* HEAD: 리소스의 헤더 취득
* OPTIONS 리소스가 서포트하는 메서드 취득
* TRACE: 루프백 시험에 사용
* CONNECT: 프록시 동작의 터널 접속으로 변경



폼에서 사용자가 입력한 데이터들을 서버로 보낼 때,
GET은 url ? 뒤에 이름=값 쌍으로 이어 붙여 보낸다. (너무 길어지면 못씀)



##### 상태 코드
* 1xx: 현재 요청까지 처리되었으니 계속 진행하라
* 2xx: 성공
* 3xx: 리다이렉션. 완전한 처리를 위해 추가적인 동작이 필요하다.
* 4xx: 클라이언트 요청 잘못
* 5xx: 서버 에러




* URL 설계: 고객의 요구사항이 정리되면, URL을 먼저 설계하게 된다. 웹 서버 로직 설계의 첫걸음.

URL은 웹 클라이언트에서 호출한다는 시점에서 보면, 웹 서버에 존재하는 애플리케이션에 대한 API라고 할 수 있다.


#####URL을 API 명명 규칙에 따라 정하는 방법
* RPC: 서버가 제공하는 API 함수를 호출하는 방식(대부분 URL 경로가 동사)
* REST: 서버에 존재하는 요소를 모두 리소스라고 정의하고, URL을 통해 웹 서버의 특정 리소스를 표현한다는 개념. 클라이언트와 서버 간 데이터 교환을 리소스 상태의 교환으로 간주한다.

간편 URL은 쿼리스트링 없이 경로만 가진 간단한 구조의 URL


#####웹 서버 vs. 웹 애플리케이션 서버

* 웹 서버: 주로 정적 페이지를 웹 클라이언트에 제공. 동적 페이지 처리가 필요하면 웹 애플리케이션 서버에 넘김.
* 웹 애플리케이션 서버: 동적 페이지 요청을 받아서 처리하고, 결과를 웹 서버로 반환.





#####정적 페이지 vs. 동적 페이지

* 정적 페이지: 항상 같은 내용을 표시하는 웹 페이지. 웹 서비스의 제공자가 사전에 준비하여 서버 측에 배치한 것
* 동적 페이지: 누가, 언제, 어떻게 요구했는지에 따라 다른 내용이 반환되는 페이지. 예) 온라인 쇼핑 몰에서의 사용자마다 다른 카트 내용

점차 동적 페이지에 대한 요구사항이 많아지면서, 웹 서버와는 다른 별도의 프로그램이 필요하게 되었다. 이러한 별도의 프로그램과 웹 서버 사이에 정보를 주고받는 규칙을 정의한 것이 바로 CGI 규격.



파이썬의 경우 mod_wsgi 모듈을 사용하고 있다. 스크립트 엔진을 웹 서버에 내장시켜 애플리케이션의 처리를 고속화함.

또 다른 방식은 애플리케이션을 처리하는 프로세스를 미리 데몬으로 기동시켜 놓은 후, 웹 서버의 요청을 데몬(사용자가 직접 제어하지 않고 백그라운드에서 돌면서 여러 작업을 하는 것)에서 처리하는 것.


웹 애플리케이션 서버에서 정적, 동적 페이지 모두 처리하는 것보단 동적 페이지만 처리하는 게 나음.




## 2장 파이썬 웹 표준 라이브러리


* urllib 패키지: 웹 클라이언트용. 주로 URL 처리와 서버 액세스 관련 API 제공.
* http 패키지: 서버용과 클라이언트용 라이브러리로 나누어 모듈을 담고 있다.

트위터 구글 같은 인터넷 서비스 제공 회사들이 외부의 프로그램들이 자신의 서비스를 사용할 수 있도록 OpenAPI를 제공하고 있으며, 이런 OpenAPI 대부분이 HTTP 프로토콜을 사용하고 있기 때문에 OpenAPI를 호출하는 프로그램도 웹 클라이언트라고 볼 수 있다.


`urlparse()` 함수는 URL을 파싱한 결과로 `ParseResult` 인스턴스를 반환한다.

```python
from urllib.parse import urlparse
result = urlparse("http://www.python.org:80/guido/python.html;philosophy?overall=3#n10")
print(result)
```

`urllib.request` 모듈: 주어진 URL에서 데이터를 가져오는 기본 기능을 제공한다. 

`urlopen()` 함수: 디폴트는 GET. 웹 서버에 전달할 파라미터가 있으면 질의 문자열을 url 인자에 포함해서 보냄. POST로 보내고 싶으면 data 인자에 포함해서 보냄.


URL로 GET/POST 방식의 간단한 요청 처리: `urlopen()` 함수’만’으로 가능

PUT, HEAD 메서드 등. 헤더 조작이 필요하면: Request 클래스를 ‘같이’ 사용.


#####GET 방식 요청
```python
from urllib.request import urlopen

f = urlopen("http://www.example.com")
print(f.read(500).decode('utf-8'))
```

HTTP GET 방식을 디폴트로 사용하여 웹 서버에 요청을 보낸다.


#####POST 방식 요청
```python
from urllib.request import urlopen
data = "language=python&framework=django"
f = urlopen("http://127.0.0.1:8000", bytes(data, encoding='utf-8'))
print(f.read(500).decode('utf-8'))
```

`urlopen()` 호출 시 `data` 인자를 지정해 주면, 자동으로 POST 방식으로 요청을 보냄.  `data`는 바이트스트링 타입이어야 한다.(유니코드 str X)



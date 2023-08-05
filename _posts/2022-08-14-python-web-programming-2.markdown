---
layout: post
title: "파이썬 웹 프로그래밍 공부 2"
date: 2022-08-14 22:17:31 +0900
tags: python-web-programming
---

요청 헤더를 지정해서 보내고 싶다면 ulr 인자에 Request 객체를 지정한다.

```python
from urllib.request import urlopen, Request
from urllib.parse import urlencode

url = 'http://127.0.0.1:8000'
data = {
   'name': '김석훈',
   'email': 'shkim@naver.com',
   'url': 'http://www.naver.com',
}

encData = urlencode(data)
postData = bytes(encData, encoding='utf-8')
req = Request(url, data=postData)
req.add_header('Content-Type', 'application/x-www-form-urlencoded')
f = urlopen(req)
print(f.info())
print(f.read(500).decode('utf-8'))
```

인증 데이터, 쿠키 데이터를 추가하여 요청을 보내는 등 HTTP의 고급 기능을 포함하여 요청을 보낼 수도 있다. 핸들러 객체를 정의하면 됨.

```python
from urllib.request import HTTPBasicAuthHandler, build_opener

auth_handler = HTTPBasicAuthHandler()
auth_handler.add_password(realm='ksh', user='shkim', passwd='shkimadmin', uri='http://127.0.0.1:8000/auth/')
opener = build_opener(auth_handler)
resp = opener.open('http://127.0.0.1:8000/auth/')
print(resp.read().decode('utf-8'))
```

쿠키 데이터를 처리하는 프로그램:

```python
from urllib.request import Request, HTTPCookieProcessor, build_opener

# 쿠키를 담기 위한 준비를 하고, 서버로 요청을 보낸다.
url = 'http://127.0.0.1:8000/cookie/'

cookie_handler = HTTPCookieProcessor()
opener = build_opener(cookie_handler)

req = Request(url)
res = opener.open(req)

print(res.info())
print(res.read().decode('utf-8'))

print("--------")

# 첫 번째 응답에서 받은 쿠키를 헤더에 담아서 요청을 보낸다.
data = "language=python&framework=django"
encData = bytes(data, encoding='utf-8')

req = Request(url, encData)
res = opener.open(req)

print(res.info())
print(res.read().decode('utf-8'))
```

프록시 서버를 통과해서 웹 서버로 요청 보내기

```python
import urllib.request

url = 'http://www.example.com'
proxyServer = 'http://www.proxy.com:3128/'

# 프록시 서버를 통해 웹 서버로 요청을 보냄
proxy_handler = urllib.request.ProxyHandler({'http': proxyServer})

# 프록시 서버 설정을 무시하고 웹 서버로 요청을 보낸다.
proxy_handler = urllib.request.ProxyHandler({})

proxy_auth_handler = urllib.request.ProxyBasicAuthHandler()
proxy_auth_handler.add_password('realm', 'host', 'username', 'password')

opener = urllib.request.build_opener(proxy_handler, proxy_auth_handler)

urllib.request.install_opener(opener)

f = urllib.request.urlopen(url)

print("geturl():", f.geturl())
print(f.read().decode('utf-8'))
```

urllib.request 구글 메인 화면의 이미지 가져오는 예제. 공부하면서 기록할 만한 내용은 주석으로 달았다.

parse_image.py

```python
from urllib.request import urlopen
from html.parser import HTMLParser

class ImageParser(HTMLParser):
   def handle_starttag(self, tag, attrs):
       if tag != 'img':
           return
       if not hasattr(self, 'result'):
           self.result = []
       for name, value in attrs:
           if name == 'src':
               self.result.append(value)


def parse_image(data):
   parser = ImageParser()
   parser.feed(data)  # HTML 문장을 feed() 함수에 주면 파싱하여 parser.result 리스트에 추가해줌.
   data_set = set(x for x in parser.result)  # 파싱 결과를 중복 없이 받는다.
   return sorted(data_set)  # 책에서는 main에서 data_set을 sort해 주는데, 여기에서 해 주는 것도 괜찮을 거 같아서 바꿈.


def main():
   url = "http://www.google.co.kr"

   with urlopen(url) as f:
       # 사이트에서 가져오는 데이터는 인코딩된 상태이므로, 인코딩 방식을 알아내어 그 방식으로 인코딩해 줍니다.
       charset = f.info().get_param('charset')
       data = f.read().decode(charset)

   data_set = parse_image(data)
   print('\n>>>>>>> Fetch Images from', url)
   print('\n'.join(data_set))


if __name__ == '__main__':
   main()
```

같은 코드를 외부 라이브러리인 requests와 beautifulsoup4로 작성한 것(부록 A)

```python
import requests
from bs4 import BeautifulSoup


def parse_image(data):
   soup = BeautifulSoup(data, 'html.parser')
   img_tag_list = soup.find_all('img')
   data_set = set(x.attrs['src'] for x in img_tag_list)
   return sorted(data_set)


def main():
   url = 'http://www.google.co.kr'

   res = requests.get(url)
   charset = res.encoding
   data = res.content.decode(charset)

   data_set = parse_image(data)
   print('\n>>>>>>> Fetch Images from', url)
   print('\n'.join(data_set))


if __name__ == '__main__':
   main()
```

http.client 모듈
`urllib.request` 모듈로는 쉽게 처리할 수 없는 경우, HTTP 프로토콜 요청에 대한 저수준의 세밀한 기능이 필요한 경우 사용.

- 연결 객체 생성
- 요청을 보냄
- 응답 객체 생성
- 응답 데이터 읽음
- 연결 닫음

http.client GET 방식으로

```python
from http.client import HTTPConnection

host = 'www.example.com'  # http:// 붙이면 오류가 발생한다.

conn = HTTPConnection(host)
conn.request('GET', '/')
r1 = conn.getresponse()

print(r1.status, r1.reason)


data1 = r1.read()
data1 = r1.read(100)

conn.request('GET', '/')
r2 = conn.getresponse()
print(r2.status, r2.reason)

data2 = r2.read()
print(data2.decode())
conn.close()
```

http.client HEAD 방식으로

```python
from http.client import HTTPConnection

conn = HTTPConnection('www.example.com')
conn.request('HEAD', '/')
resp = conn.getresponse()
print(resp.status, resp.reason)

data = resp.read()
print(len(data))
print(data == b'')
```

http.client 모듈을 사용하여 POST 메서드로 요청 보내기

```python
from http.client import HTTPConnection
from urllib.parse import urlencode

host = '127.0.0.1:8000'
params = urlencode({
   'language': 'python',
   'name': '김석훈',
   'email': 'shkim@naver.com',
})
headers = {
   'Content-Type': 'application/x-www-form-urlencoded',
   'Accept': 'text/plain'
}

conn = HTTPConnection(host)
conn.request('POST', '', params, headers)
resp = conn.getresponse()
print(resp.status, resp.reason)

data = resp.read()
print(data.decode('utf-8'))

conn.close()
```

http.client 모듈 사용하여 PUT 메서드로 요청 보내기

```python
from http.client import HTTPConnection
from urllib.parse import urlencode

host = '127.0.0.1:8000'
params = urlencode({
   'language': 'python',
   'name': '김석훈',
   'email': 'shkim@naver.com',
})
headers = {
   'Content-Type': 'application/x-www-form-urlencoded',
   'Accept': 'text/pain',
}

conn = HTTPConnection(host)
conn.request('PUT', '', params, headers)
resp = conn.getresponse()
print(resp.status, resp.reason)

data = resp.read()
print(data.decode('utf-8'))

conn.close()
```

http.client 모듈을 이용한 이미지 다운로드하는 코드

```python
import os.path
from http.client import HTTPConnection
from urllib.parse import urljoin, urlunparse
from urllib.request import urlretrieve
from html.parser import HTMLParser


class ImageParser(HTMLParser):
   """img 태그를 파서하는 듯?"""
   def handle_starttag(self, tag: str, attrs):
       if tag != 'img':
           return
       if not hasattr(self, 'result'):
           self.result = []
       for name, value in attrs:
           if name == 'src':
               self.result.append(value)


def download_image(url, data):
   """HTML 문장이 주어지면 ImageParser 클래스를 이용해서 이미지를 찾고, 그 이미지들을 DOWNLOAD 디렉토리에 다운로드한다."""

   if not os.path.exists('DOWNLOAD'):
       os.makedirs('DOWNLOAD')

   parser = ImageParser()
   parser.feed(data)  #parser.result에 결과 넣어줌
   dataSet = set(x for x in parser.result)

   for x in sorted(dataSet):
       imageUrl = urljoin(url, x)  # url과 타깃 파일명 지정함.
       basename = os.path.basename(imageUrl)
       targetFile = os.path.join('DOWNLOAD', basename)

       print('Downloading...', imageUrl)
       urlretrieve(imageUrl, targetFile)  # image 다운로드 함수.


def main():
   host = 'www.google.co.kr'

   conn = HTTPConnection(host)
   conn.request('GET', '')
   resp = conn.getresponse()

   charset = resp.msg.get_param('charset')
   data = resp.read().decode(charset)
   conn.close()

   print("\n>>>>> Download Images from", host)
   url = urlunparse(('http', host, '', '', '', ''))
   download_image(url, data)


if __name__ == '__main__':
   main()
```

http.client 모듈 예제 beautifulsoup으로 재작성(부록A)

```python
import requests
import os
from urllib.parse import urljoin
from bs4 import BeautifulSoup


def parse_image(data):
   soup = BeautifulSoup(data, 'html.parser')
   imgTagList = soup.find_all('img')
   dataSet = set(x.attrs['src'] for x in imgTagList)
   return sorted(dataSet)


def download_image(url, dataSet):

   if not os.path.exists('DOWNLOAD'):
       os.makedirs('DOWNLOAD')

   for x in dataSet:
       imageUrl = urljoin(url, x)
       basename = os.path.basename(imageUrl)
       targetFile = os.path.join('DOWNLOAD', basename)

       print('Downloading...', imageUrl)
       res = requests.get(imageUrl)
       with open(targetFile, 'wb') as f:
           f.write(res.content)


def main():
   url = "http://www.google.co.kr"

   res = requests.get(url)
   charset = res.encoding
   data = res.content.decode(charset)

   dataSet = parse_image(data)

   print('\n>>>>>> Download Images from', url)
   download_image(url, dataSet)


if __name__ == '__main__':
   main()
```

웹 서버 라이브러리

웹 서버의 역할: http 통신에서 클라이언트의 요청을 받고 이를 처리하여 결과를 되돌려주는 것

간단하지만, 웹 서버를 만드는 가장 기본적인 방법을 따른 것

```python
from http.server import HTTPServer, BaseHTTPRequestHandler

class MyHandler(BaseHTTPRequestHandler):
   """웹 클라이언트로부터 요청을 받고 'Hello World'라는 문장을 되돌려 주는 웹 서버"""
   def do_GET(self):
       self.send_response_only(200, 'OK')
       self.send_header('Content-Type', 'text/plain')
       self.end_headers()
       self.wfile.write(b'Hello World')


if __name__ == '__main__':
   server = HTTPServer(('', 8888), MyHandler)  # HTTPServer 객체 생성(인자: 서버의 IP, PORT, 핸들러 클래스)
   print('Started WebServer on port 8888...')
   print('Press ^C to quit WebServer.')
   server.serve_forever()
```

`HTTPServer`와 `BaseHTTPRequestHandler`가 기반이 되는 클래스. HTTP 프로토콜을 처리해 주는 기능이 있어서 이 클래스들을 상속받으면 관련 로직을 코딩하지 않아도 된다.

CGI 웹 서버용 CGI 스크립트

```python
import cgi

form = cgi.FieldStorage()  # FieldStorage 클래스의 인스턴스를 생성해야 한다.
name = form.getvalue('name')  # 그리고 그 인스턴스의 getvalue() 메서드를 호출해야 한다.
email = form.getvalue('email')
url = form.getvalue('url')

print('Content-Type: text/plain')
print()

print('Welcome... CGI Scripts')
print('name is', name)
print('email is', email)
print('url is', url)
```

CGI 웹 서버 시험용 클라이언트

```python
from urllib.request import urlopen
from urllib.parse import urlencode

url = 'http://127.0.0.1:8888/cgi-bin/script.py'
data = {
   'name': '김석훈',
   'email': 'shkim@naver.com',
   'url': 'http://www.naver.com'
}

enc_data = urlencode(data)
post_data = enc_data.encode('ascii')

f = urlopen(url, post_data)
print(f.read().decode('cp949'))
```

#### xxxHTTPServer 모듈 간의 관계

모든 HTTP 웹 서버는 BaseHTTPServer 모듈의 HTTPServer 클래스를 사용하여 작성된다.
웹 서버에 사용되는 핸들러는 BaseHTTPServer 모듈의 BaseHTTPRequestHandler를 상속 받아 작성

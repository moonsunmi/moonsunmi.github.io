---
layout: post
title: "HTTP Server 만들기 - index, 404 page 추가해 보기"
date: 2023-09-20 12:18:07 +0900
tags: Python server
---

HTTP의 정의를 다시 한번 살펴보고 시작한다.

HTTP는 통신 규약이다. 서버와 클라이언트의 통신에 지켜야 할 규약이다. 이는 텍스트 패턴을 지키는 것으로 얻을 수 있다. HTTP의 구조에서 Header에는 어떤 내용이 포함되어 있고, body는 어떻게 구성되고 하는 것들이 지켜져야 할 텍스트 구성을 나타내는 것이다.

다음은 GET 메서드로 통신했을 때 얻을 수 있는 간단한 HTTP 예제이다.

```
GET /index.html HTTP/1.0
User-Agent: Mozilla/5.0

```

header와 body 사이에는 공백이 있어야 하므로 공백 한 줄까지 포함되어 있다.

내가 만드는 HTTP Sever도 이 텍스트 패턴을 지키도록 해야 한다.

### 계속 동작하는 서버 만들기

앞 글에서 만들었던 기초적인 서버는 클라이언트 요청을 한 번 처리하면 바로 종료되었다. 진짜 서버처럼 동작하도록 수정한다.

```python
import socket

SERVER_HOST = '0.0.0.0'
SERVER_PORT = 8080

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
server_socket.bind((SERVER_HOST, SERVER_PORT))
server_socket.listen(1)

try:
    while True:
        client_conn, client_addr = server_socket.accept()

        request = client_conn.recv(1024).decode()
        print(request)

        response = 'HTTP/1.0 200 OK\nContent-Type: text/html\n\nHello, World'
        client_conn.sendall(response.encode())
        client_conn.close()

finally:
    server_socket.close()


```

다음 코드는 새롭게 추가된 것이다.

```python
socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
```

서버가 비정상적으로 종료되었을 때를 대비해서 소켓 옵션을 설정한 것이다.

서버가 클라이언트에게 보낼 때, `send()` 대신 `sendall()`을 사용했는데, 이렇게 하면 에러가 나거나 모든 데이터를 다 보낼 때까지 send를 계속 진행한다.

```python
client_conn.sendall(response.encode())
```

앞에서는 클라이언트를 만들었지만, 이번에는 브라우저를 통해서 접속해 본다. 브라우저는 HTTP 사용자 에이전트이므로, 서버의 데이터를 받아서 알아서 사용자에게 보여준다.

### 서버가 가져간 클라이언트의 정보

웹 브라우저를 통해 클라이언트가 접속하면, 클라이언트의 다음 정보를 서버에서 출력한다.

```
GET / HTTP/1.1
Host: localhost:8080
Connection: keep-alive
Cache-Control: max-age=0
sec-ch-ua: "Not.A/Brand";v="8", "Chromium";v="114", "Google Chrome";v="114"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "macOS"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) ...
...
```

### 서버에서 index.html을 띄우게 하기

이제 이 서버에 접속하면 index.html을 띄우도록 해 보자. 클라이언트에 응답할 때 index.html 파일 내용에 헤더를 붙여서 전달해 주면 된다. 내가 만드는 html은 `pages` 안에 저장하도록 했다.

```python
fin = open('pages/index.html')
content = fin.read()
fin.close()

response = 'HTTP/1.0 200 OK\nContent-Type: text/html\n\n' + content
```

### 루트 페이지 외의 나머지 페이지 띄워 주기

페이지를 이동한면 다른 페이지를 띄워 줘야 한다. request 맨 윗줄의 `GET` 다음에 있는 `/`가 경로 정보이다.

```
GET / HTTP/1.1
```

만약 `http://localhost:8080/hello.html` 같은 식으로 접속한다면 `/` 대신 `/hello.html`가 전달된다. 그러므로 이 부분을 파싱하여 열어줄 파일 이름에 매칭시킨다.

그런데 존재하지 않는 경로로 접근할 경우 다음과 같은 에러가 발생하면서 서버가 종료된다. 따라서 예외 처리도 함께 해 준다.

```
FileNotFoundError: [Errno 2] No such file or directory: 'pages/hello'
```

```python
try:
    fin = open('pages' + filename)
    content = fin.read()
    fin.close()
    response = 'HTTP/1.1 200 OK\nContent-Type: text/html\n\n' + content

except FileNotFoundError:
    response = 'HTTP/1.1 404 NOT FOUND\nContent-Type: text/html\n\nFile Not Found'
```

#### 웹 브라우저 외의 접근 처리하기

그런데 서버를 띄워두고, 클라이언트에서 직접 뭔가를 요청하지 않았는데도 다음과 같이 parsing 에러가 발생하기도 했다.

```
    filename = headers[0].split()[1]
IndexError: list index out of range
```

네트워크 스캔 도구 등 웹 브라우저 외의 다른 곳에서도 이 서버에 접근하기도 하는데, 이때 정상적인 HTTP 요청 형식을 따르지 않기도 하기 때문이다. 따라서 이런 상황에서의 처리도 다음과 같이 해 주었다.

```python
request = client_conn.recv(1024).decode()
headers = request.split('\n')

if len(headers) > 1:
    first_header_parts = headers[0].split()

    if len(first_header_parts) > 1:
        ...

    else:
        print("Invalid HTTP Request line")
else:
    print("Empty Request")
```

### 간단한 서버 프로그램

간단한 서버 프로그램을 다음과 같이 완성했다. favicon.ico를 자동으로 호출하게 되는데, 보통은 static 폴더 안에 저장한다. 여기서는 간단하게 서버를 만들어 볼 목적이었으므로, favicon.ico에 대한 호출을 무시하도록 했다.

```python
import socket

SERVER_HOST = '0.0.0.0'
SERVER_PORT = 8080

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
server_socket.bind((SERVER_HOST, SERVER_PORT))
server_socket.listen(1)

try:
    while True:
        client_conn, client_addr = server_socket.accept()

        request = client_conn.recv(1024).decode()
        headers = request.split('\n')

        if len(headers) > 1:
            first_header_parts = headers[0].split()

            if len(first_header_parts) > 1:
                if filename == '/':
                    filename = '/index.html'

                if filename != '/favicon.ico':
                    try:
                        fin = open('pages' + filename)
                        content = fin.read()
                        fin.close()
                        response = 'HTTP/1.1 200 OK\nContent-Type: text/html\n\n' + content

                    except FileNotFoundError:
                        response = 'HTTP/1.1 404 NOT FOUND\nContent-Type: text/html\n\nFile Not Found'

            else:
                print("Invalid HTTP Request line")
        else:
            print("Empty Request")

        client_conn.sendall(response.encode())
        client_conn.close()

finally:
    server_socket.close()
```

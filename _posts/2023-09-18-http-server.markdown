---
layout: post
title: "HTTP Server 만들기 - 기초적인 네트워크 소켓 프로그래밍"
date: 2023-09-18 12:18:07 +0900
tags: Python server
---

HTTP는 통신 규약이다. 쉽게 말해, 서버와 클라이언트 사이에서 통신을 하기 위해 필요한 약속이다. 이 약속은 특정 패턴을 지키는 텍스트로 구현되어 있다.

### 네트워크 소켓

네트워크 애플리케이션(네트워크 통신을 이용하는 모든 애플리케이션)을 만들려면 네트워크 소켓이 필요하다. 네트워크 소켓은 운영체제에서 지원하는 추상화된 인터페이스로, 네트워크를 통해 바이트 단위의 데이터를 주고받을 수 있게 한다. 통신 간의 endpoint로(흔히 창구로 비유를 많이 한다), 통신을 하려면 반드시 소켓을 열고, 통신하려는 곳의 소켓과 연결되어야 한다.

### 서버 프로그램

클라이언트는 소켓을 만들고 연결만 하면 되는 반면(`socket()`, `connect()`) 서버는 `socket()`, `bind()`, `listen()`, `accept()`을 해야 한다.

#### 서버 소켓 만들기

소켓을 만들 때는 다음과 같이 하면 된다.

```
import socket

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
```

여기서 `AF_INET`은 주소 체계를 나타낸다. 우리가 대부분 IPv4를 사용하기는 하지만, 다른 주소 체계도 존재하므로(IPv6 등) 이에 대한 언급을 해주는 것이다. `AF_INET`은 IPv4를 나타낸다. IPv6를 사용하고 싶다면 `AF_INET6`를 쓴다.

#### 서버에 연결 가능한 주소 지정하기

IPv4 주소용으로 만든 소켓을 패밀리 주소와 연결해야 한다. 이게 binding 작업.

```
server_socket.bind(('', 8080))
```

안 쓰는(비어 있는) 포트 번호에 소켓을 연결시켜 주었다. 패밀리 주소에 빈칸을 넣어 두었는데, 빈칸은 모든 네트워크 인터페이스를 뜻한다. 즉, 어떤 클라이언트라도 8080 포트 번호를 통해 해당 소켓에 연결될 수 있다는 이야기다. 여기에서 바인딩 인자는 튜플 형태로 받으므로, `('', 8080)` 자체를 넣어주어야 한다는 걸 잊지 말자. (코드에서 보이기에 괄호 두 개)

#### 서버를 연결 가능한 상태로 만들기

이제 서버를 연결 가능한 상태로 만든다. 다음과 같이 `listen()`을 이용한다.

```
socket_server.listen(1)
```

여기에서 `1`은 backlog값을 뜻한다. 여러 클라이언트가 동시에 접속하는 상황에서, 서버가 한번에 처리하지 못한 일이 발생했을 때 이 작업을 몇 개까지 큐에 넣어둘 것인지를 지정하는 것이다. 0보다 적은 숫자를 입력하면, 0으로 세팅하고, 숫자를 지정하지 않는다면 파이썬이 알아서 적정 값을 정한다.

이제 서버는 연결 가능한 상태로, 클라이언트를 기다리게 된다.

#### 실제 연결

서버는 `accept()` 전까지의 작업을 한 후 클라이언트를 기다리고 있다가 클라이언트가 접속해 오면, 그제서야 `accept()`가 반환하는 값을 전달해 준다. 클라이언트가 서버에 시도하는 연결을 말 그대로 수락하는 것이다.

```
conn, addr = socket_server.accept()
```

`accept()`가 반환하는 것은 연결이 된 새로운 소켓과 그 소켓에 연결된 클라이언트 주소이다. `socket_server`는 실제 연결이 되기 전까지 대기하는 소켓이었고, `accept()`가 실제 연결된 소켓을 반환하였으니 연결된 상태의 소켓을 사용하려면 이제 `conn`을 사용하면 된다.

### 클라이언트 프로그램

클라이언트는 소켓을 만든 후 `connect()`만 해 주면 된다.

```
import socket

client_socket = socket.socket(AF_INET, SOCK_STREAM)
client_socket.connect(('127.0.0.1', 8080))
```

`127.0.0.1`은 나 자신을 의미하므로, 내가 8080 포트를 이용해서 서버 프로그램에 접속하겠다는 뜻이 된다.

### 소켓을 통해 데이터 송수신하기

데이터를 주고 받을 때는 `recv()`와 `send()`를 이용하면 된다. 상단에서 언급하였듯이 소켓은 '바이트 단위의 데이터'를 주고받을 수 있게 한다. 따라서 파이썬 오브젝트인 데이터는 꼭 바이트 단위 형태로 바꾸어 주어야 한다.

```
msg = 'hi'
conn.send(msg.encode('utf-8'))
```

받을 때는 다음과 같이 한다.

```
msg = conn.recv(1024)
print(msg.decode('utf-8'))
```

encode했으니 decode하는 건 이해가 가는데, `recv(1024)`라는 게 눈에 띈다.

통신에 시간이 걸릴 수도 있고 사이즈가 매우 클 수도 있기 때문에, `recv()`는 데이터를 몇 번씩 나눠서 가져온다. 여기서 1024는 `bufsize`를 나타내며, 한번에 가져올 수 있는 최대의 바이트 수를 나타낸다.

### 샘플 코드 만들기

서버와 클라이언트가 다음과 같이 통신하는 프로그램이다. 서버는 클라이언트의 주소를 받아 다음과 같이 출력한다.

```
sever:
I'm connected with  ('127.0.0.1', 51467)
```

클라이언트는 자신이 보낸 메시지에 'You said that'라고 붙여 반응하는 서버의 메시지를 출력한다.

```
client:
You say that Hello? anyone is there? I am client
```

코드는 다음과 같다.

```
sever.py

import socket

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind(('', 8080))
server_socket.listen(1)
conn, client_addr = server_socket.accept()
received_msg = conn.recv(1024)
server_msg = 'You say that '
conn.send(server_msg.encode('utf-8') + received_msg)
print("I'm connected with ", client_addr)
```

```
client.py

import socket

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(('127.0.0.1', 8080))
msg = 'Hello? anyone is there? I am client'
client_socket.send(msg.encode('utf-8'))
sever_msg = client_socket.recv(1024)
print(sever_msg.decode('utf-8'))

```

서버를 실행하면, 아무것도 출력되지 않는다. 클라이언트를 기다리고 있는 것이다. 클라이언트를 실행하면 그제서야 결과물을 출력하고 종료한다. 클라이언트는 즉시 결과물을 출력하고 종료한다.

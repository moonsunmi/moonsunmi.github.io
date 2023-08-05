---
layout: post
title: "연오의 파이썬 공부 18"
date: 2022-08-10 18:18:22 +0900
tags: yeono-python
---

코드 12-29

```python
import urllib.request

TOKEN = '1178934754:AAGUR5T93yTA7ncs_r3m0b_Hxw8h_8fb4y0'

def request(url):
   """지정한 url의 웹 문서를 요청하여, 본문을 반환한다."""
   response = urllib.request.urlopen(url)
   byte_data = response.read()
   text_data = byte_data.decode()
   return text_data


def build_url(method, query):
   """텔레그램 챗봇 웹 API에 요청을 보내기 위한 URL을 만들어 반환한다."""
   return f'http://api.telegram.org/bot{TOKEN}/{method}?{query}'


import json
from pprint import pprint

def request_to_chatbot_api(method, query):
   """메서드와 질의조건을 전달받아 텔레그램 챗봇 웹 API에 요청을 보내고 응답 결과를 사전 객체로 해석해 반환한다."""
   url = build_url(method, query)
   response = request(url)
   return json.loads(response)


response = request_to_chatbot_api('getUpdates', 'offset=0')
pprint(response)
```

코드 12-31

```python
import urllib.request

TOKEN = '1178934754:AAGUR5T93yTA7ncs_r3m0b_Hxw8h_8fb4y0'

def request(url):
   """지정한 url의 웹 문서를 요청하여, 본문을 반환한다."""
   response = urllib.request.urlopen(url)
   byte_data = response.read()
   text_data = byte_data.decode()
   return text_data


def build_url(method, query):
   """텔레그램 챗봇 웹 API에 요청을 보내기 위한 URL을 만들어 반환한다."""
   return f'http://api.telegram.org/bot{TOKEN}/{method}?{query}'


import json
from pprint import pprint

def request_to_chatbot_api(method, query):
   """메서드와 질의조건을 전달받아 텔레그램 챗봇 웹 API에 요청을 보내고 응답 결과를 사전 객체로 해석해 반환한다."""
   url = build_url(method, query)
   response = request(url)
   return json.loads(response)


response = request_to_chatbot_api('getUpdates', 'offset=0')
#pprint(response)


def simplify_message(response):

   result = response['result']

   if not result:
       return None, []

   last_update_id = max(item['update_id'] for item in result)
   messages = [item['message'] for item in result]
   simplified_messages = [
       {'from_id': message['message_id'],
       'text': message['text']} for message in messages
   ]

   return last_update_id, simplified_messages


print(simplify_message(response))
```

코드 12-33

```python
import urllib.request
import json

TOKEN = '1178934754:AAGUR5T93yTA7ncs_r3m0b_Hxw8h_8fb4y0'

def request(url):
   """지정한 url의 웹 문서를 요청하여, 본문을 반환한다."""
   response = urllib.request.urlopen(url)
   byte_data = response.read()
   text_data = byte_data.decode()
   return text_data


def build_url(method, query):
   """텔레그램 챗봇 웹 API에 요청을 보내기 위한 URL을 만들어 반환한다."""
   return f'http://api.telegram.org/bot{TOKEN}/{method}?{query}'


def request_to_chatbot_api(method, query):
   """메서드와 질의조건을 전달받아 텔레그램 챗봇 웹 API에 요청을 보내고 응답 결과를 사전 객체로 해석해 반환한다."""
   url = build_url(method, query)
   response = request(url)
   return json.loads(response)


def simplify_message(response):

   result = response['result']

   if not result:
       return None, []

   last_update_id = max(item['update_id'] for item in result)
   messages = [item['message'] for item in result]
   simplified_messages = [
       {'from_id': message['message_id'],
       'text': message['text']} for message in messages
   ]

   return last_update_id, simplified_messages


def get_updates(update_id):
   """챗봇 API로 update_id 이후에 수신한 메시지를 조회하여 반환한다."""
   query = f'offset={update_id}'
   response = request_to_chatbot_api(method='getUpdates', query=query)
   return simplify_message(response)


response = request_to_chatbot_api('getUpdates', 'offset=0')
last_update_id, simplified_message = simplify_message(response)
print(simplified_message)
print(get_updates(32))
```

코드 12-36

```python
import urllib.request
import json

TOKEN = '1178934754:AAGUR5T93yTA7ncs_r3m0b_Hxw8h_8fb4y0'

def request(url):
   """지정한 url의 웹 문서를 요청하여, 본문을 반환한다."""
   response = urllib.request.urlopen(url)
   byte_data = response.read()
   text_data = byte_data.decode()
   return text_data


def build_url(method, query):
   """텔레그램 챗봇 웹 API에 요청을 보내기 위한 URL을 만들어 반환한다."""
   return f'http://api.telegram.org/bot{TOKEN}/{method}?{query}'


def request_to_chatbot_api(method, query):
   """메서드와 질의조건을 전달받아 텔레그램 챗봇 웹 API에 요청을 보내고 응답 결과를 사전 객체로 해석해 반환한다."""
   url = build_url(method, query)
   response = request(url)
   return json.loads(response)


def simplify_message(response):

   result = response['result']

   if not result:
       return None, []

   last_update_id = max(item['update_id'] for item in result)
   messages = [item['message'] for item in result]
   simplified_messages = [
       {'from_id': message['message_id'],
       'text': message['text']} for message in messages
   ]

   return last_update_id, simplified_messages


def get_updates(update_id):
   """챗봇 API로 update_id 이후에 수신한 메시지를 조회하여 반환한다."""
   query = f'offset={update_id}'
   response = request_to_chatbot_api(method='getUpdates', query=query)
   return simplify_message(response)


response = request_to_chatbot_api('getUpdates', 'offset=0')
last_update_id, simplified_message = simplify_message(response)
print(simplified_message)
print(get_updates(32))
text = urllib.parse.quote('안녕~~')
request_to_chatbot_api('sendMessage', f'chat_id=219819940&text={text}')
```

코드 12-41

```python
import urllib.request
import time
import urllib.parse
import json

TOKEN = '1178934754:AAGUR5T93yTA7ncs_r3m0b_Hxw8h_8fb4y0'


def request(url):
   """지정한 url의 웹 문서를 요청하여, 본문을 반환한다."""
   response = urllib.request.urlopen(url)
   byte_data = response.read()
   text_data = byte_data.decode()
   return text_data


def build_url(method, query):
   """텔레그램 챗봇 웹 API에 요청을 보내기 위한 URL을 만들어 반환한다."""
   return f'http://api.telegram.org/bot{TOKEN}/{method}?{query}'


def request_to_chatbot_api(method, query):
   """메서드와 질의조건을 전달받아 텔레그램 챗봇 웹 API에 요청을 보내고 응답 결과를 사전 객체로 해석해 반환한다."""
   url = build_url(method, query)
   response = request(url)
   return json.loads(response)


def simplify_message(response):

   result = response['result']

   if not result:
       return None, []

   last_update_id = max(item['update_id'] for item in result)
   messages = [item['message'] for item in result]
   simplified_messages = [
       {'from_id': message['message_id'],
       'text': message['text']} for message in messages
   ]

   return last_update_id, simplified_messages


def get_updates(update_id):
   """챗봇 API로 update_id 이후에 수신한 메시지를 조회하여 반환한다."""
   query = f'offset={update_id}'
   response = request_to_chatbot_api(method='getUpdates', query=query)
   return simplify_message(response)


def send_message(chat_id, text):
   """챗봇 API로 메시지를 chat_id 사용자에게 text 메시지를 발신한다."""
   text = urllib.parse.quote(text)
   query = f'chat_id={chat_id}&text={text}'
   response = request_to_chatbot_api(method='sendMessage', query=query)
   return response


def check_messages_and_response(next_update_id):
   """챗봇으로 메시지를 확인하고, 적절히 응답한다."""
   last_update_id, received_messages = get_updates(next_update_id)
   for message in received_messages:
       chat_id = message['from_id']
       text = message['text']
       send_text = text + '라고 말씀하셨군요~'
       send_message(chat_id, send_text)
   return last_update_id


if __name__ == '__main__':
   next_update_id = 0
   while True:
       last_update_id = check_messages_and_response(next_update_id)
       if last_update_id:
           next_update_id = last_update_id + 1
       time.sleep(5)


response = request_to_chatbot_api('getUpdates', 'offset=0')
send_message(219819940, '안뇽')
```

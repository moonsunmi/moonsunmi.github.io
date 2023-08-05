---
layout: post
title: "연오의 파이썬 공부 15"
date: 2022-08-07 22:13:19 +0900
tags: yeono-python
---

현재 시간 측정 후 해석

```python
import time
now = time.gmtime(time.time())
```

시간 단위 다룰 때 많이 쓰는 datetime 모듈

```python
from datetime import time
time(15, 30, 45) # 15시 30분 45초
```

```
from datetime import datetime
datetime(2020, 11, 14, 8, 30)  # 2020년 11월 14일 8시 30분
```

```
now.isoformat()
'2022-08-07T06:44:30.304039'
```

날짜와 시간을 `T`로 구별. `now.isoformat((sep=' '))`와 같이 매개변수로 지정할 수 있다.

`strftime()` 메서드는 양식 문자열을 전달받아 시간 정보 객체를 양식화한다.

코드 11-45

```python
from datetime import timedelta
timedelta(days=5)
datetime.timedelta(days=5)
timedelta(weeks=5)
datetime.timedelta(days=35)
```

연습문제 11-3 -> 논리 오류: 윤년을 생각 못하고, 1년 365일로 계산하였음.

```python
from datetime import date


class BirthdayException(Exception):
   """생일 오류"""
   pass


def counting_age(birth_date):
   """생년월일(birth_date)을 입력받아 세는 나이를 반환한다."""
   # 세는 나이는 year 계산
   year_gap = date.today().year - birth_date.year
   if year_gap < 0:
       raise BirthdayException('미래에 태어날 사람의 나이는 계산할 수 없습니다.')
   return year_gap + 1


def full_age(birth_date):
   """생년월일(birth_date)을 입력받아 만 나이를 반환한다."""
   # 만 나이는 day 계산
   day_gap = date.today() - birth_date
   if day_gap.days < 0:
       raise BirthdayException('미래에 태어날 사람의 나이는 계산할 수 없습니다.')
   return day_gap.days // 365


def test_full_age():
   assert full_age(date(2022, 8, 7)) == 0
   assert full_age(date(2020, 1, 1)) == 2
   assert full_age(date(2020, 12, 31)) == 1


def test_counting_age():
   assert counting_age(date(2022, 8, 7)) == 1
   assert counting_age(date(2020, 1, 1)) == 3
   assert counting_age(date(2020, 12, 31)) == 3


test_full_age()
test_counting_age()
try:
   print('만 나이: ', full_age(date(1985, 7, 16)))
except Exception as error:
   print(error)

try:
   print('세는 나이: ', counting_age(date(1985, 7, 16)))
except Exception as error:
   print(error)

try:
   print('만 나이: ', full_age(date(2985, 7, 16)))
except Exception as error:
   print(error)

try:
   print('세는 나이: ', counting_age(date(2985, 7, 16)))
except Exception as error:
   print(error)
```

연습문제 11-3 update

```python
# 논리 오류: 윤년을 생각 못하고, 1년을 365일로 고정하여 계산함.

from datetime import date, timedelta


class BirthdayException(Exception):
   """생일 오류"""
   pass


def counting_age(birth_date):
   """생년월일(birth_date)을 입력받아 세는 나이를 반환한다."""
   # 세는 나이는 year 계산
   year_gap = date.today().year - birth_date.year
   if year_gap < 0:
       raise BirthdayException('미래에 태어날 사람의 나이는 계산할 수 없습니다.')
   return year_gap + 1


def full_age(birth_date):
   """생년월일(birth_date)을 입력받아 만 나이를 반환한다."""
   # 만 나이는 day 계산
   day_gap = date.today() - birth_date

   if day_gap.days < 0:
       raise BirthdayException('미래에 태어날 사람의 나이는 계산할 수 없습니다.')

   is_after_birthday = timedelta(0) <= date.today() - date(date.today().year, birth_date.month, birth_date.day)

   return date.today().year - birth_date.year + (0 if is_after_birthday else -1)


def test_full_age():

   assert full_age(date(1999, 1, 1)) == 23
   assert full_age(date(1999, 8, 8)) == 22
   assert full_age(date(2000, 1, 1)) == 22
   assert full_age(date(2000, 8, 8)) == 21

   assert full_age(date(2020, 1, 1)) == 2
   assert full_age(date(2020, 12, 31)) == 1
   assert full_age(date(2022, 8, 7)) == 0


def test_counting_age():
   assert counting_age(date(2022, 8, 7)) == 1
   assert counting_age(date(2020, 1, 1)) == 3
   assert counting_age(date(2020, 12, 31)) == 3

   assert counting_age(date(1999, 1, 1)) == 24
   assert counting_age(date(1999, 8, 8)) == 24
   assert counting_age(date(2000, 1, 1)) == 23
   assert counting_age(date(2000, 8, 8)) == 23


test_full_age()
test_counting_age()
try:
   print('만 나이: ', full_age(date(1985, 7, 16)))
except Exception as error:
   print(error)

try:
   print('세는 나이: ', counting_age(date(1985, 7, 16)))
except Exception as error:
   print(error)

try:
   print('만 나이: ', full_age(date(2985, 7, 16)))
except Exception as error:
   print(error)

try:
   print('세는 나이: ', counting_age(date(2985, 7, 16)))
except Exception as error:
   print(error)
```

리스트·사전 등 컬렉션을 중첩한 입체적인 데이터를 파일로 저장하고 읽어 들이기 위해
약속에 따라 입체적인 데이터를 텍스트나 바이트로 표현하는 것을 직렬화(serialization)라고 한다.

코드 11-50

```python
print('당신의 이름은 무엇인가요?')
name = input()

with open('hello.txt', 'w') as file:
   print(name, '님 반가워요.', file=file)
```

코드 11-51

```python
from datetime import date
today = date.today()

text = f'안녕하세요! 오늘은 {today.month}월 {today.day}일입니다.\n'

with open('hello.txt', 'w') as file:
   file.write(text)
```

코드 11-52

```python
from datetime import datetime
now = datetime.now()

text = f'현재 시각: {now.hour:02}:{now.minute:02}:{now.second:02}\n'

with open('time.txt', 'a') as file:
   file.write(text)
```

코드 11-53

```python
try:
   with open('once.text', 'x') as file:
       file.write('이 파일은 한 번만 작성됩니다.')
except FileExistsError:
   print('파일이 이미 존재합니다.')
```

코드 11-54

```python
with open('time.txt', 'r') as file:
   date = file.read()

print(date)
```

코드 11-55

```python
with open('time.txt', 'r') as file:
   for line in file:
       print(line, end='')
```

코드 11-56

```python
import csv
import pprint

with open('movies.csv') as file:
   reader = csv.reader(file)
   movies = list(reader)

pprint.pprint(movies)
```

코드 11-57

```python
import csv

table = [
   ['title', 'genre', 'year'],
   ['Interstellar', 'SF', '2014'],
   ['Braveheart', 'Drama', '1995'],
   ['Mary Poppins', 'Fantasy', '1964'],
   ['Gloomy Sunday', 'Drama', '2000'],
]

with open('movies_output.csv', 'w', newline='') as file:
   writer = csv.writer(file)
   writer.writerows(table)
```

코드 11-58

```python
import json

data = [
   {
       'title': 'Interstellar',
       'genre': 'SF',
       'year': 2014,
       'starring': ['M. McConaughey', 'A. Hathaway', 'J. Chastain'],
   },
   {
       'title': 'Mary Poppins',
       'genre': 'Fantasy',
       'year': 1964,
       'starring': ['J. Andrews', 'D. Van Dyke'],
   }
]

json_data = json.dumps(data)
print(json_data)
with open('movies.json', 'w') as file:
   file.write(json_data)
```

코드 11-59

```python
import json
import pprint

with open('movies.json') as file:
   json_data = file.read()

data = json.loads(json_data)
pprint.pprint(data)



코드 11-62
from pathlib import Path
def ls(path):
   path_obj = Path(path)

   for item in path_obj.iterdir():
       print(item)

print(ls('/Users')) # macOS라서 다르게 실습
```

코드 11-63

```python
from pathlib import Path
path = Path('/Users')
path_list = list(path.iterdir())
print(path_list)
print(path_list[0])
print(path_list[0].is_dir())
```

코드 11-65

```python
from pathlib import Path
path = Path('/Users')
path_list = list(path.iterdir())

Path('dir1').mkdir()
Path('dir2').mkdir()
Path('file1').touch()

Path('dir2').replace('dir1/dir2')
Path('dir1/dir2').replace(Path('dir2'))
Path('dir2').replace('dir3')
```

코드 11-66

```python
from pathlib import Path
def mv(src, dst):
   """src 경로의 파일을 dst 경로로 이동한다."""
   src_obj = Path(src)
   dst_obj = Path(dst)

   if dst_obj.exists():
       raise FileExistsError
   src_obj.replace(dst_obj)

mv('dir2', 'dir88')
```

연습문제 11-4

```python
from pathlib import Path


def cp(src, dst):
   src_obj = Path(src)
   dst_obj = Path(dst)

   if not src_obj.exists():
       raise FileNotFoundError

   with open(src_obj, 'r') as file:
       all_text = file.read()

   with open(dst_obj, 'w') as file:
       file.write(all_text)


cp('original.txt', 'clone.txt')
```

연습문제 11-4 모범답안처럼 업데이트

```python
def cp(src, dst):

   with open(src, 'r') as read_file, open(dst, 'w') as write_file:
       write_file.write(read_file.read())


cp('original.txt', 'clone.txt')
```

연습문제 11-5

```python
import csv

# 인구 밀도가 높은 나라부터 낮은 나라 순으로 나라 이름과 인구 밀도를 출력하라.


def population_density(population, area):
   """인구 밀도를 구한다."""
   return population / area


with open('population.csv', 'r') as file:
   reader = csv.reader(file)
   countries = list(reader)[1:]

country_density = []
for country in countries:
   country_density.append([country[0], population_density(int(country[1]), int(country[2]))])

sorted_country_density = sorted(country_density, key=lambda country_density: country_density[1], reverse=True)

for density in sorted_country_density:
   print(density[0], density[1])
```

연습문제 11-5 모범답안처럼 업데이트

```python
import csv


# 인구 밀도가 높은 나라부터 낮은 나라 순으로 나라 이름과 인구 밀도를 출력하라.
def population_density(population, area):
   """인구 밀도를 구한다."""
   return population / area


with open('population.csv', 'r') as file:
   reader = csv.reader(file)
   countries = list(reader)[1:]

for country in countries:
   country.append(population_density(int(country[1]), int(country[2])))

sorted_country = sorted(countries, key=lambda row: row[3], reverse=True)

for density in sorted_country:
   print(density[0], density[3])
```

## 부록 A

HTTP 통신의 과정

1. 웹브라우저가 en.wikipedia.org 호스트에 /wiki/Python\_(programming_language)라는 자원을 요청하는 HTTP 요청 메시지를 발송한다.
2. en.wikipedia.org 호스트가 클라이언트에 HTTP 응답 메시지를 발송한다.
3. 웹브라우저가 응답 메시지를 해석하여 화면에 웹 문서를 출력한다.

파이썬은 URL과 웹 요청에 관련된 모듈들을 urllib 패키지도 묶어 제공한다.

`urllib.request.urlopen(요청할URL)`: 응답객체(HTTPResponse)를 반환한다.
`urllib.request.urlopen(요청할URL).read()`: 웹서버가 응답한 데이터를 바이트 배열로 읽어 들임
`urllib.request.urlopen(요청할URL).read().decode('utf-8')`: 바이트 배열을 문자열로 변환

HTTPS는 HTTP에 SSL이라는 암·복호화 단계를 적용하여 보안 통신을 수행하는 프로토콜

코드 11-68

```python
import urllib.request
def request(url):
   """지정한 url의 웹 문서를 요청하여, 본문을 반환한다."""
   response = urllib.request.urlopen(url)
   byte_date = response.read()
   text_data = byte_date.decode('utf-8')
   return text_data
```

코드 11-69

```python
import urllib.request

url = 'https://python.bakyeono.net'
webpage = urllib.request.urlopen(url).read().decode('utf-8')
print(webpage)
```

코드 11-70

```python
import urllib.request

url = 'https://python.bakyeono.net/data/movies.json'
text_data = urllib.request.urlopen(url).read().decode('-utf-8')
print(text_data)
```

코드 11-71

```python
import urllib.request
import json

url = 'https://python.bakyeono.net/data/movies.json'
text_data = urllib.request.urlopen(url).read().decode('-utf-8')
movies = json.loads(text_data)
sorted_by_year = sorted(movies, key=lambda movie: movie['year'])

for movie in sorted_by_year:
   print(str(movie['year']) + ' ' + movie['title'].upper())
```

코드 11-72

```python
import urllib.parse

url = 'https://python.bakyeono.net/data/movies.json'
url_parts = urllib.parse.urlsplit(url)
print(url_parts[0])
print(url_parts[1])
print(url_parts[2])
```

코드 11-73

```python
import urllib.parse

url = 'https://python.bakyeono.net/data/movies.json'
url_parts = urllib.parse.urlsplit(url)
url_parts = list(url_parts)
url_parts[2] = '/chapter-11.html'
print(urllib.parse.urlunsplit(url_parts))
```

URL에는 영문자, 숫자, 몇몇 기호들만 사용할 수 있다. 아스키 코드가 아닌 문자들을 퍼센트 인코딩 형식으로 바꾸어야 한다.

http.server 모듈로 간단한 웹 서버를 만들 수 있다.

코드 11-78

```python
import http.server


class HTTPRequestHandler(http.server.BaseHTTPRequestHandler):

   def do_GET(self):
       """HTTP GET 요청을 처리한다."""
       self.route()

   def route(self):
       """요청 URL의 path에 따라 요청을 처리할 함수를 중계한다."""
       if self.path == '/hello':
           self.hello()
       else:
           self.response_404_not_found()

   def hello(self):
       """200 OK 상태 코드와 인사말을 응답한다."""
       self.response(200, '안녕하세요?')

   def response(self, status_code, body):
       """응답 메시지를 전송한다."""
       self.send_response(status_code)

       self.send_header('Content-type', 'text/plain; charset=utf-8')
       self.end_headers()

       self.wfile.write(body.encode('utf-8'))


ADDRESS = 'localhost', 8000

listener = http.server.HTTPServer(ADDRESS, HTTPRequestHandler)
print(f'http://{ADDRESS[0]}:{ADDRESS[1]} 주소에서 요청 대기중...')
listener.serve_forever()
```

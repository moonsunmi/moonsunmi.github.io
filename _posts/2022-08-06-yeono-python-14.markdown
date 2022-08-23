---
layout: post
title:  "연오의 파이썬 공부 14"
date:   2022-08-06 22:21:11 +0900
categories: yeono-python
---




* 로그: 프로그램을 실행하는 도중에 화면, 파일, 데이터 베이스 등의 장소에 하는 중간 기록

* 단위테스트: 프로그램의 구성요소를 테스트하는 것을 단위 테스트


예외적인 데이터를 빠트리지 않고 테스트하는 게 중요하다.
> 주의<br>
> 문자열을 입력받는 함수는 빈 문자열, 매우 긴 문자열, 외국어, 특수문자, 홀수 또는 짝수 길이의 문자열
정수를 입력받는 함수는 0, 음수, 매우 작거나 매우 큰 수, 0부터 세는가, 1부터 세는가




#### 테스트 주도 개발
함수를 작성하기 전에 테스트를 먼저 작성하는 방법. 함수의 입력값과 출력값의 사례를 미리 만들어 두는 것.


코드 9-16
```python
def frequency(s, c):
   """문자열 s와 문자 c를 입력받아, c가 s에 등장하는 빈도를 구한다.
   테스트:
   * 'banana', 'a' => 0.5
   * 'code', 'c' => 0.25
   * '파이썬', '프' => 0
   * '파이썬', '파이' => None
   * '', 'a' => 0
   """
   if len(c) != 1:
       return None

   if len(s) == 0:
       return 0

   count = s.count(c)
   return count / len(s)


def test_frequency():
   """frequency() 함수 테스트"""
   if not (frequency('banana', 'a') == 0.5):
       return False

   if not(frequency('code', 'c') == 0.25):
       return False

   if not(frequency('파이썬','프') == 0):
       return False

   if not(frequency('파이썬', '파이') == None):
       return False

   if not (frequency('', 'a') == 0):
       return False

   return True


print(test_frequency())
```




연습문제 9-1
```python
def median(data):
   """데이터의 중앙값을 반환한다."""
   sorted_data =sorted(data)
   median_value = sorted_data[int(len(sorted_data) / 2)]
   return median_value

print(median([10, 9, 4, 1, 5, 7]))

# list 인덱스가 실수형이 되었기 때문에 오류가 났었다.(지금은 수정)
```



연습문제 9-2
```python
def 삼육구(n):
   """n에 숫자 '3', '6', '9' 중 하나 이상이 있을 경우 '짝'을,
   그렇지 않으면 n에 대응하는 숫자를 반환한다."""
   characters = str(n)
   found_3 = characters.find('3') != -1
   found_6 = characters.find('6') != -1
   found_9 = characters.find('9') != -1
   if found_3 or found_6 or found_9:
       return '짝'
   else:
       return str(n)


def test_삼육구():
   if not 삼육구(0) == '0':
       return False

   if not 삼육구(8) == '8':
       return False

   if not 삼육구(3) == '짝':
       return False

   if not 삼육구(6) == '짝':
       return False

   if not 삼육구(9) == '짝':
       return False

   if not 삼육구(120) == '120':
       return False

   if not 삼육구(300) == '짝':
       return False

   if not 삼육구(66669333) == '짝':
       return False

   if not 삼육구(2481112) == '2481112':
       return False

   if not 삼육구(-10) == '-10':
       return False

   if not 삼육구(-9) == '짝':
       return False

   return True


print(test_삼육구())
```



연습문제 9-3

수를 0으로 나누려고 할 때 발생하는 오류.
if 문 등을 이용해서 나눗수가 0이 될 경우 예외 처리를 해 준다.




코드 9-20
```python
while True:
   print('0이 아닌 정수를 입력해 주세요:', end=' ')
   user_string = input()

   if not user_string.isnumeric():
       print(user_string, '은 정수가 아닙니다.')
       continue

   user_number = int(user_string)

   if user_number == 0:
       print('0으로 나눌 수 없습니다.')
       continue

   break

print( 1 / user_number)
```




> if 문을 이용한 예외처리 단점
> 1. 예외를 나타내는 값(오류 코드)과 정상 값을 구별하기가 어렵다. 
> 2. 함수를 연달아 호출할 때, 예외를 함수 밖으로 전달하기가 불편하다. 
> 3. 예외 상황인지 항상 미리 검사해야 한다.



예외를 나타내는 값을 정해 둔 것을 오류 코드(error code)라 한다. 오류 코드는 함수마다 제각각 정의하게 될 가능성이 높다. 



파이썬은 허락을 구한 뒤 일하기보다는, 일을 먼저 하고 문제가 생기면 용서를 구하는 방법을 권장한다.



코드 9-24
```python
try:
   print('0이 아닌 정수를 입력해 주세요:', end=' ')
   user_number = int(input())
   print(1 / user_number)

except ZeroDivisionError:
   print('0으로 나눌 수 없습니다.')

except ValueError:
   print('입력한 값은 정수가 아닙니다.')
```



코드 9-25
```python
while True:
   try:
       print('0이 아닌 정수를 입력해 주세요:', end=' ')
       user_number = int(input())
       print(1 / user_number)
       break
   except ZeroDivisionError:
       print('0으로 나눌 수 없습니다.')

   except ValueError:
       print('입력한 값은 정수가 아닙니다.')
```



try 문의 else절을 쓰는 이유는 예외처리의 대상이 되는 코드와 이어서 수행될 코드를 명시적으로 구별하기 위해서다.



* 311p 중간 ~ 312p 중간까지 실습 아직 x

코드 9-27

try문을 사용하면 안쪽에서 발생한 오류를 바깥에서 처리할 수 있다.


연습문제 9-4
```python
try:
   print('첫 번째 수를 입력하시오: ', end='')
   number1 = int(input())
   print('두 번째 수를 입력하시오: ', end='')
   number2 = int(input())
   add = number1 + number2
   subtract = number1 - number2
   multiply = number1 * number2

   print(number1, '+', number2, '=', add)
   print(number1, '-', number2, '=', subtract)
   print(number1, '*', number2, '=', multiply)

   divide = number1 / number2

except ZeroDivisionError:
   print(number1, '/', number2, '=', None)

else:
   print(number1, '/', number2, '=', divide)


연습문제 9-4 upgrade

try:
   print('첫 번째 수를 입력하시오: ', end='')
   number1 = int(input())
   print('두 번째 수를 입력하시오: ', end='')
   number2 = int(input())
   add = number1 + number2
   subtract = number1 - number2
   multiply = number1 * number2
   divide = number1 / number2

except ZeroDivisionError:
   divide = None

   print(number1, '+', number2, '=', add)
   print(number1, '-', number2, '=', subtract)
   print(number1, '*', number2, '=', multiply)
   print(number1, '/', number2, '=', divide)

```



파이썬에는 다양한 오류 상황이 예외 클래스로 범주화되어 있다. 예외가 발생하면 예외 상황에 대한 정보를 담은 예외 인스턴스가 만들어진다. 새로운 예외의 종류를 직접 정의하거나 예외 객체를 직접 생성할 수도 있다.


코드 9-32
```python
def get(index_or_key, collection):
   """컬렉션(collection)에서 인덱스 또는 키(index_or_key)에 해당하는 값을 반환한다.
   데이터 집합에 해당하는 인덱스 또는 키가 존재하지 않는 경우 None을 반환한다."""

   try:
       value = collection[index_or_key]
   except (IndexError, KeyError):
       return None
   else:
       return value

print(get(3, (1, 2, 3)))
```


코드 9-33
```python
def get(index_or_key, collection):
   """컬렉션(collection)에서 인덱스 또는 키(index_or_key)에 해당하는 값을 반환한다.
   데이터 집합에 해당하는 인덱스 또는 키가 존재하지 않는 경우 None을 반환한다."""

   try:
       value = collection[index_or_key]
   except LookupError:
       return None
   else:
       return value

print(get(3, (1, 2, 3)))
```



as를 이용하면 except 절 안에서 예외 객체에 담긴 오류 정보를 이용할 수 있다.




파이썬에 기본으로 정의되어 있는 예외는 인터프리터가 적절한 상황에서 자동으로 발생
하지만 오류 상황을 직접 정의하여 예외를 발생시켜야 할 때는 raise 문 or assert 문 이용



코드 9-41
```python
class DoorException(Exception):
   """문 관련 예외"""
   pass
class DoorOpenedException(DoorException):
   """문 열림 예외"""
   pass
class DoorClosedException(DoorException):
   """문 닫힘 예외"""
   pass

class Door:
   """문을 나타내는 클래스"""
   def __init__(self):
       self._is_opened = True

   def open(self):
       if self._is_opened:
           raise DoorOpenedException('문이 이미 열려 있음')
       else:
           print('문을 엽니다.')
           self._is_opened = True

   def close(self):
       if not self._is_opened:
           raise DoorClosedException('문이 이미 닫혀 있음')
       else:
           print('문을 닫습니다.')
           self._is_opened = False

door = Door()
door.close()
door.open()
door.open()
```

raise 문은 오류가 이미 발생한 상황에서 예외를 일으키기 위한 명령. 따라서 무조건 예외를 일으킴
assert 문은 상태를 검증하기 위한 명령. 지정한 검증식을 계산하여 결과가 거짓일 때만 AssertionError 예외를 발생시킨다.



코드 9-44
```python
class DoorException(Exception):
   """문 관련 예외"""
   pass
class DoorOpenedException(DoorException):
   """문 열림 예외"""
   pass
class DoorClosedException(DoorException):
   """문 닫힘 예외"""
   pass

class Door:
   """문을 나타내는 클래스"""
   def __init__(self):
       self._is_opened = True

   def open(self):
       assert not self._is_opened

       print('문을 엽니다.')
       self._is_opened = True

   def close(self):
       assert self._is_opened

       print('문을 닫습니다.')
       self._is_opened = False

door = Door()
door.close()
door.open()
door.open()
```



연습문제 9-5
```python
class AccountException(Exception):
   """은행 계좌 관련 예외"""
   pass

class AccountBalanceException(AccountException):
   """계좌 잔고 예외"""
   pass

class FrozenAccountException(AccountException):
   """동결 계좌 예외"""
   pass

class InvalidTransactionException(AccountException):
   """잘못된 입출금 예외"""
   pass
```



연습문제 9-6
```python
class AccountException(Exception):
   """은행 계좌 관련 예외"""
   pass

class AccountBalanceException(AccountException):
   """계좌 잔고 예외"""
   pass

class FrozenAccountException(AccountException):
   """동결 계좌 예외"""
   pass

class InvalidTransactionException(AccountException):
   """잘못된 입출금 예외"""
   pass


class Account():
   """은행 계좌"""

   def __init__(self, balance, is_frozen):
       """인스턴스를 초기화한다."""
       self.balance = balance  # 계좌 잔액
       self.is_frozen = is_frozen  # 계좌 동결 여부

   def check(self):
       """계좌의 잔고를 조회한다."""

       print('계좌 잔액은', self.balance, '원입니다.')

   def deposit(self, amount):
       """계좌에 amount만큼의 금액을 입금한다."""

       if self.is_frozen:
           raise FrozenAccountException

       if not amount > 0:
           raise InvalidTransactionException('0 이하의 액수는 입금할 수 없습니다.')

       self.balance += amount

   def withdraw(self, amount):
       """계좌에서 amount만큼의 금액을 인출한다."""
       if self.is_frozen:
           raise FrozenAccountException

       if not amount > 0:
           raise InvalidTransactionException('0 이하의 액수는 인출할 수 없습니다.')

       if self.balance < amount:
           raise AccountBalanceException('잔액이 부족합니다.')
       else:
           self.balance -= amount


account_1 = Account(1000, False)

try:
   account_1.deposit(1000)
except Exception as error:
   print(error)

try:
   account_1.withdraw(-1000)
except Exception as error:
   print(error)

try:
   account_1.check()
except Exception as error:
   print(error)
```



연습문제 9-7

```python
class AccountException(Exception):
   """은행 계좌 관련 예외"""
   pass

class AccountBalanceException(AccountException):
   """계좌 잔고 예외"""
   pass

class FrozenAccountException(AccountException):
   """동결 계좌 예외"""
   pass

class InvalidTransactionException(AccountException):
   """잘못된 입출금 예외"""
   pass


class Account():
   """은행 계좌"""

   def __init__(self, balance, is_frozen):
       """인스턴스를 초기화한다."""
       self.balance = balance  # 계좌 잔액
       self.is_frozen = is_frozen  # 계좌 동결 여부

   def check(self):
       """계좌의 잔고를 조회한다."""

       print('계좌 잔액은', self.balance, '원입니다.')

   def deposit(self, amount):
       """계좌에 amount만큼의 금액을 입금한다."""

       assert amount > 0, '거래 금액이 잘못되었습니다.'

       self.balance += amount

   def withdraw(self, amount):
       """계좌에서 amount만큼의 금액을 인출한다."""
       assert not self.is_frozen, '계좌가 동결되었습니다.'
       assert amount > 0, '거래 금액이 잘못되었습니다.'
       assert self.balance >= amount, '잔액이 부족합니다.'

       self.balance -= amount


account_1 = Account(1000, False)

try:
   account_1.deposit(1000)
except Exception as error:
   print(error)

try:
   account_1.withdraw(-1000)
except Exception as error:
   print(error)

try:
   account_1.check()
except Exception as error:
   print(error)
```

디스크, 네트워크, 프린터 등의 시스템 자원은 직접 신경 써서 반납해야 한다.



코드 9-45
```python
total = 0
file = open('prices.txt')
for line in file:
   total += int(line)
print(total)
file.close()
```


코드 9-46
```python
try:
   total = 0
   file = open('prices.txt')
   for line in file:
       total += int(line)
except ValueError:
   print('숫자가 아닌 텍스트가 있습니다.')
else:
   print(total)
   file.close()
```

finally 절: 예외 발생 여부와 관계 없이 항상 마지막에 실행된다.


코드 9-47
```python
try:
   total = 0
   file = open('prices.txt')
   for line in file:
       total += int(line)
except ValueError:
   print('숫자가 아닌 텍스트가 있습니다.')
else:
   print(total)
finally:
   file.close()
```


with 문: 뒷정리를 자동으로 처리하는 문법



코드 9-48
```python
with open('prices.txt') as file:
   try:
       total = 0
       file = open('prices.txt')
       for line in file:
           total += int(line)
   except ValueError:
       print('숫자가 아닌 텍스트가 있습니다.')
   else:
       print(total)
```

with 문은 객체의 종류에 관계없이 알아서 준비와 뒷정리를 해 준다.
준비를 위해 `__enter__()` 메서드를 호출, 뒷정리를 위해 `__exit()__` 메서드를 알아서 호출한다.


코드 9-49
```python
class NetworkConnection:
   """네트워크 연결을 나타내는 클래스"""

   def __init__(self, url):
       """인스턴스를 초기화한다."""

       self.url = url
       self.is_connected = False

   def connect(self):
       """네트워크에 연결한다."""

       print('네트워크에 연결합니다.')
       self.is_connected = True

   def disconnect(self):
       """네트워크 연결을 중단한다."""

       print('네트워크 연결을 중단합니다.')
       self.is_connected = False

   def read(self):
       """네트워크에서 데이터를 읽어 들인다."""

       return '네트워크에서 읽어 들인 데이터'

   def __enter__(self):
       """객체 사용을 준비한다."""

       self.connect()
       return self

   def __exit__(self, exc_type, exc_value, traceback):
       """객체 사용을 마치고 뒷정리한다."""

       self.disconnect()


with NetworkConnection('https://bakyeono.net') as connection:
   print(connection.read())
```




## 10장



모듈(module)은 프로그램을 구성하는 기능 중에서 독립적으로 구별할 수 있는 것을 모아 분리해 둔 것이다.


프로그램 실행의 시작점이 되는 모듈을 최상위(top-level) 모듈이라 한다. 



`__name__` 전역 변수는 모듈이 최상위로 실행된 경우에는 `__main__`이 되고, 하위 모듈로 실행된 경우에는 그 모듈의 이름이 된다.


파이썬의 패키지(package)는 여러 개의 모듈·패키지를 묶은 것이다.





* 코드 10-16, 코드 10-17 실습 아직 하지 못함.





각 패키지 디렉터리마다 있는 `__init__.py` 파일의 역할: 파이썬이 디렉터리를 패키지로 인식하도록 한다.





model/__init__.py
```python
—-------------------------
from . import character
```

현재 패키지인 `model`을 기준으로 character 패키지를 상대경로로 임포트하라는 뜻이다.




## 11장


라이브러리(library)란 다른 프로그램의 구성요소로 사용하기 위해 미리 만들어 둔 프로그램 조각이다. 즉, 자주 사용하는 기능을 모듈·패키지로 만들어 둔 것이다. 



연습문제 11-1
```python
import math


def floor_division(dividen, divisor):
   return math.floor(dividen / divisor)


def test_floor_division():
   if not floor_division(3, 2) == 1: # x.5
       return False

   if not floor_division(3, 3) == 1:  # x.5 미만
       return False

   if not floor_division(5, 3) == 1:  # x.5 초과 (무한 소수)
       return False

   if not floor_division(12, 3) == 4:  # x.0
       return False

   return True


print(test_floor_division())

# 모범답안처럼 assert 문을 사용했으면 더 간단하게 했을 텐데!
```


코드 11-17
```python
print('{0[0]}년 {0[1]}월 {0[2]}일 {1[0]}시 {1[1]}분'.format([2020, 6, 20], [3, 30]))
```


코드 11-18
```python
print('{0[h]}시 {0[m]}분 {0[s]}초'.format({'h': 16, 'm': 30, 's': 0}))
```




코드 11-20
```python
countries = [
   {'name': 'China', 'population': 1403500365},
   {'name': 'Japan', 'population': 126056362},
   {'name': 'South Korea', 'population': 51736224},
   {'name': 'Pitcairn Islands', 'population': 56},
]
form = '나라: {0} | 인구: {1}'
for country in countries:
   print(form.format(country['name'], country['population']))
```


코드 11-21
```python
countries = [
   {'name': 'China', 'population': 1403500365},
   {'name': 'Japan', 'population': 126056362},
   {'name': 'South Korea', 'population': 51736224},
   {'name': 'Pitcairn Islands', 'population': 56},
]
form = '나라: {0:16} | 인구: {1:10}'
for country in countries:
   print(form.format(country['name'], country['population']))
```


문자열은 왼쪽, 수는 오른쪽 정렬이 디폴트

코드 11-22
```python
countries = [
   {'name': 'China', 'population': 1403500365},
   {'name': 'Japan', 'population': 126056362},
   {'name': 'South Korea', 'population': 51736224},
   {'name': 'Pitcairn Islands', 'population': 56},
]
form = '나라: {0:16} | 인구: {1:010}'
for country in countries:
   print(form.format(country['name'], country['population']))
```


코드 11-23
```python
countries = [
   {'name': 'China', 'population': 1403500365},
   {'name': 'Japan', 'population': 126056362},
   {'name': 'South Korea', 'population': 51736224},
   {'name': 'Pitcairn Islands', 'population': 56},
]
form = '나라: {0:>16} | 인구: {1:<10}'
for country in countries:
   print(form.format(country['name'], country['population']))
```



코드 11-25
```python
physics, chemistry, biology = 80, 90, 70
print(f'물리학: {physics}, 화학: {chemistry}, 생물학: {biology}')
```


코드 11-26
```python
for i in range(2, 10):
   for j in range(1, 10):
       print(f'{i} ** {j} = {i ** j:10}')
```

텍스트의 패턴을 나타내는 텍스트를 정규식(regular expression)이라 한다.


코드 11-27
```python
import re
print(re.search('파이썬', '파이썬'))
print(re.search('파이썬', '즐거운 파이썬'))
print(re.search('파이썬', '파이프'))
```


코드 11-28
```python
import re
match = re.search('파이썬', '파이썬 프로그래밍')
if match:
   print('문자열에 패턴과 매치하는 텍스트가 존재함:', match.group())
else:
   print('문자열에 패턴과 매치하는 텍스트가 존재하지 않음')

```




코드 11-29
```python
import re
print(re.sub('파이썬', '리스프', '즐거운 파이썬 프로그래밍'))
print(re.sub(' ', '*', '즐거운 파이썬 프로그래밍'))
```


코드 11-31
```python
import re
sample = '1789Pthon박연오fog'
pattern = r'[가-힣]{2,4}'
match = re.search(pattern, sample)
print(match.group())
```


코드 11-32
```python
import re
sample = '이름: 박연오, 연락처: 010-1234-5678, 주소: 부산 어딘가'
pattern = r'01\d-\d{3,4}-\d{4}'
match = re.search(pattern, sample)
print(match.group())
```



연습문제 11-2
```python
import re


def hangul_only(string):
   pattern = r'[^가-힣]+'
   only_hangul_match = re.split(pattern, string)

   result = ''
   for hangul_word in only_hangul_match:
       result += hangul_word
   return result


print(hangul_only('I like 파이썬 programming.'))
print(hangul_only('a1가b2나c3다d4라e5마f6바g7사'))
```



연습문제 11-2 모범답안처럼 업그레이드

```python
import re


def hangul_only(string):
   pattern = r'[^가-힣]+'
   only_hangul = re.sub(pattern, '', string)

   return only_hangul


def test_hangul_only():
   assert hangul_only('') == ''
   assert hangul_only('안녕') == '안녕'
   assert hangul_only('123') == ''
   assert hangul_only('안녕 00 나의 vv 친구')


test_hangul_only()
print(hangul_only('I like 파이썬 programming.'))
print(hangul_only('a1가b2나c3다d4라e5마f6바g7사'))
```



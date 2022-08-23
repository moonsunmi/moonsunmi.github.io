---
layout: post
title:  "연오의 파이썬 공부 11"
date:   2022-08-03 22:17:42 +0900
categories: yeono-python
---






코드 8-4
```python
coordinate_2 = (3, 5)
triangle_2 = ((0, 0), (0, 3), (4, 3))
rectangle_2 = {
   'point': (2, 2),
   'width': 4,
   'height': 4,
}
```


코드 8-5
```python
# 오류가 발생하는 예제
import math

coordinate_2 = (3, 5)
triangle_2 = ((0, 0), (0, 3), (4, 3))
rectangle_2 = {
   'point': (2, 2),
   'width': 4,
   'height': 4,
}


def square(x):
   """전달받은 수의 제곱을 반환한다."""
   return x * x


def distance(point_a, point_b):
   return math.sqrt(square(point_a['x'] - point_b['x']) + square(point_a['y'] - point_b['y']))


def circumference_of_triangle(shape):
   """삼각형 데이터를 전달받아 둘레를 구해 반환한다."""
   a_to_b = distance(shape['point_a'], shape['point_b'])
   b_to_c = distance(shape['point_b'], shape['point_c'])
   c_to_a = distance(shape['point_c'], shape['point_a'])

   return a_to_b + b_to_c + c_to_a


print(circumference_of_triangle(triangle_2))
```



동일한 범주에 속하는 데이터의 유형을 정의하고 그 구조를 동일하게 구성해야 한다.


코드 8-6
```python
# 유형: '좌표'는 다음 키를 가지는 사전이다.
#   * 'x': x축의 위치 (정수)
#   * 'y': y축의 위치 (정수)
coordinate_1 = {'x': 5, 'y': 3}

# 유형: '삼각형'은 다음 키를 가지는 사전이다.
#   * 'point_a': 첫 번째 점의 위치(좌표)
#   * 'point_b': 두 번째 점의 위치(좌표)
#   * 'point_c': 세 번째 점의 위치(좌표)
triangle_1 = {
   'point_a': {'x': 0, 'y': 0},
   'point_b': {'x': 3, 'y': 0},
   'point_c': {'x': 3, 'y': 4},
}


# 유형: '사각형'은 다음 키를 가지는 사전이다.
#   * 'point_a': 첫 번째 점의 위치(좌표)
#   * 'point_b': 두 번째 점의 위치(좌표)
#   * 'point_c': 세 번째 점의 위치(좌표)
#   * 'point_d': 첫 번째 점의 위치(좌표)
rectangle_1 = {
   'point_a': {'x': 2, 'y': 2},
   'point_b': {'x': 6, 'y': 2},
   'point_c': {'x': 6, 'y': 6},
   'point_d': {'x': 2, 'y': 6},
}

import math


def square(x):
   """전달받은 수의 제곱을 반환한다."""
   return x * x


def distance(point_a, point_b):
   return math.sqrt(square(point_a['x'] - point_b['x']) + square(point_a['y'] - point_b['y']))


def circumference_of_triangle(shape):
   """삼각형 데이터를 전달받아 둘레를 구해 반환한다."""
   a_to_b = distance(shape['point_a'], shape['point_b'])
   b_to_c = distance(shape['point_b'], shape['point_c'])
   c_to_a = distance(shape['point_c'], shape['point_a'])

   return a_to_b + b_to_c + c_to_a


def circumference_of_rectangle(shape):
   """사각형 데이터를 전달받아 둘레를 구해 반환한다."""
   a_to_b = distance(shape['point_a'], shape['point_b'])
   b_to_c = distance(shape['point_b'], shape['point_c'])
   c_to_d = distance(shape['point_c'], shape['point_d'])
   d_to_a = distance(shape['point_d'], shape['point_a'])

   return a_to_b + b_to_c + c_to_d + d_to_a


print(circumference_of_triangle(triangle_1))
print(circumference_of_rectangle(rectangle_1))
```




코드 8-9
```python
import math


def square(x):
   """전달받은 수의 제곱을 반환한다."""
   return x * x


def distance(point_a, point_b):
   return math.sqrt(square(point_a['x'] - point_b['x']) + square(point_a['y'] - point_b['y']))


def circumference(shape):
   """도형 데이터를 전달받아 둘레를 구해 반환한다."""

   if type(shape) == '삼각형':
       a_to_b = distance(shape['point_a'], shape['point_b'])
       b_to_c = distance(shape['point_b'], shape['point_c'])
       c_to_a = distance(shape['point_c'], shape['point_a'])
       return a_to_b + b_to_c + c_to_a

   elif type(shape) == '사각형':
       a_to_b = distance(shape['point_a'], shape['point_b'])
       b_to_c = distance(shape['point_b'], shape['point_c'])
       c_to_d = distance(shape['point_c'], shape['point_d'])
       d_to_a = distance(shape['point_d'], shape['point_a'])
       return a_to_b + b_to_c + c_to_d + d_to_a

   else:
       return None



triangle_1 = {
   'point_a': {'x': 0, 'y': 0},
   'point_b': {'x': 3, 'y': 0},
   'point_c': {'x': 3, 'y': 4},
}

rectangle_1 = {
   'point_a': {'x': 2, 'y': 2},
   'point_b': {'x': 6, 'y': 2},
   'point_c': {'x': 6, 'y': 6},
   'point_d': {'x': 2, 'y': 6},
}

print(circumference(triangle_1))
print(circumference(rectangle_1))

print(type(triangle_1))
print(type(rectangle_1))
```





코드 8-11

```python
import math


def square(x):
   """전달받은 수의 제곱을 반환한다."""
   return x * x


def distance(point_a, point_b):
   return math.sqrt(square(point_a['x'] - point_b['x']) + square(point_a['y'] - point_b['y']))


def circumference(shape):
   """도형 데이터를 전달받아 둘레를 구해 반환한다."""

   if shape['type'] == '삼각형':
       a_to_b = distance(shape['point_a'], shape['point_b'])
       b_to_c = distance(shape['point_b'], shape['point_c'])
       c_to_a = distance(shape['point_c'], shape['point_a'])
       return a_to_b + b_to_c + c_to_a

   elif shape['type'] == '사각형':
       a_to_b = distance(shape['point_a'], shape['point_b'])
       b_to_c = distance(shape['point_b'], shape['point_c'])
       c_to_d = distance(shape['point_c'], shape['point_d'])
       d_to_a = distance(shape['point_d'], shape['point_a'])
       return a_to_b + b_to_c + c_to_d + d_to_a

   else:
       return None


triangle_3 = {
   'type': '삼각형',
   'point_a': {'x': 0, 'y': 0},
   'point_b': {'x': 3, 'y': 0},
   'point_c': {'x': 3, 'y': 4},
}
rectangle_3 = {
   'type': '사각형',
   'point_a': {'x': 2, 'y': 2},
   'point_b': {'x': 6, 'y': 2},
   'point_c': {'x': 6, 'y': 6},
   'point_d': {'x': 2, 'y': 6},
}


print(circumference(triangle_3))
print(circumference(rectangle_3))
```











개선할 점:
데이터 유형을 `type()` 함수로 식별할 수 있도록
데이터 유형마다 그에 맞는 함수를 별도로 정의하되, 그 함수가 그 데이터 유형에서만 실행될 수 있도록 강제하자


연습문제 8-1
```python
# 체스말 데이터 유형 정의하기
#   'x': x축의 위치
#   'y': y축의 위치
#   'color': 체스말의 색깔
#   'role': 체스말의 역할

체스말1 = {'x': 'A', 'y': '8', 'color': 'black', 'role': '룩'}
체스말2 = {'x': 'E', 'y': '1', 'color': 'white', 'role': '킹'}


# 바둑돌 데이터 유형 정의하기
#   'x': x축의 위치
#   'y': y축의 위치
#   'order': 바둑돌이 놓인 수
#   'color': 바둑돌의 색깔

바둑돌1 = {'x': 8, 'y': 14, 'order': 83, 'color': '흑'}
바둑돌2 = {'x': 12, 'y': 3, 'order': 84, 'color': '백'}
```




연습문제 8-2
```python
# 체스말 데이터 유형 정의하기
#   'x': x축의 위치
#   'y': y축의 위치
#   'color': 체스말의 색깔
#   'role': 체스말의 역할

체스말1 = {'type': '체스말', 'x': 'A', 'y': '8', 'color': 'black', 'role': '룩'}
체스말2 = {'type': '체스말', 'x': 'E', 'y': '1', 'color': 'white', 'role': '킹'}


# 바둑돌 데이터 유형 정의하기
#   'x': x축의 위치
#   'y': y축의 위치
#   'order': 바둑돌이 놓인 수
#   'color': 바둑돌의 색깔

바둑돌1 = {'type': '바둑돌', 'x': 8, 'y': 14, 'order': 83, 'color': '흑'}
바둑돌2 = {'type': '바둑돌', 'x': 12, 'y': 3, 'order': 84, 'color': '백'}


def print_piece(piece):
   """말을 전달받아 종류에 따라 다른 형태로 위치를 출력한다."""

   if piece['type'] == '체스말':
       print(piece['color'] + ' 룩이 ' + piece['x'] + piece['y'] + '위치에 놓여 있어요.')
   elif piece['type'] == '바둑돌':
       print('제 ', piece['order'], ' 수: ', piece['color'], '을 (',
             piece['x'], ', ', piece['y'], ') 위치에 두었습니다.', sep='')


print_piece(체스말1)
print_piece(바둑돌2)
```




### 클래스와 객체

`type()` 함수가 반환하는 데이터는 type이라는 데이터 유형. 데이터 유형을 나타내기 위한 데이터가 존재한다. 이 데이터가 바로 클래스이다. 0,1,2,3의 데이터 유형이 정수인 것처럼, int, float, str의 데이터 유형은 클래스이다.


컴퓨터에서 정보는 메모리 위에 올린 후 처리한다. 메모리 위에 올려둔 정보를 의미 있는 덩어리로 묶어 두기 위한 단위가 필요하다. 파이썬에서는 객체 단위로 메모리 위의 정보를 관리한다.

객체에는 값, 유형, 정체성이라는 세 특성이 있다. 
값은 메모리에 기록된 내용이다.
유형은 데이터의 종류로, 유형에 따라 그 값을 어떻게 읽고 다루어야 할지가 결정된다. 
정체성은 각각의 객체를 식별하기 위한 고유번호로, 객체가 메모리 속에서 위치한 주소 값이기도 하다. 



동일한 객체를 다른 변수에 대입하면, 두 변수가 같은 객체를 가리킨다.
```python
year = 1789
same_year = year
id(year) == id(same_year)  => True
```


객체가 어떤 클래스에 속할 때, 그  객체를 해당 클래스의 인스턴스라 한다. 1,2,3은 모두 int 클래스의 인스턴스다.


검사하려는 객체와 클래스가 정해져 있을 때, `isinstance()` 함수로 객체가 그 클래스에 속하는지 검사할 수 있다. 한 객체는 동시에 여러 클래스의 인스턴스일 수 있다.



파이썬의 모든 데이터는 객체이고, 모든 객체는 어떤 유형에 속한다. 즉, 파이썬의 모든 데이터는 인스턴스이다. 파이썬에서 데이터를 생성하는 것은 곧 인스턴스를 생성하는 것이다. 

인스턴스 만들기1: 그대로 평가하기. 리터럴
인스턴스 만들기2: 식을 평가하기.
인스턴스 만들기3: 인스턴스화하기. 클래스 이름에 괄호를 붙여서 수행. ex) `int(1789.0)` 정수 인스턴스 만들기



연습문제 8-3

a. 아니다. 값이 같을 뿐 객체가 같으려면 정체성이 같아야 한다.
b. 아니다. 유형이 같을 뿐. 
c. 맞다.
d. 아니다. 같은 객체를 가리켜야 함.




연습문제 8-4
```python
()
{}
print(type({range(10)}))
int(input())
str(1789)
bool([])
```




클래스 정의하기

코드 8-22
```python
class Cake:
   """케이크를 나타내는 클래스"""

   coat = '생크림'

cake_1 = Cake()
cake_2 = Cake()

print(Cake)
print(type(cake_1))
print(isinstance(cake_2, Cake))
```


속성은 일반적인 변수와 같은데, 클래스와 인스턴스 속에 정의되는 것이 특징이다.
클래스의 속성은 클래스 객체뿐 아니라 그 클래스의 모든 인스턴스가 공유한다.

코드 8-23
```python
class Cake:
   """케이크를 나타내는 클래스"""

   coat = '생크림'

cake_1 = Cake()
cake_2 = Cake()

print(Cake.coat)
print(cake_1.coat)
print(cake_2.coat)
```



코드 8-24
```python
​​class Cake:
   """케이크를 나타내는 클래스"""

   coat = '생크림'

cake_1 = Cake()
cake_2 = Cake()


Cake.coat = '초콜릿'
Cake.price = 4000
print(Cake.coat)
print(Cake.price)

print(cake_1.coat)
print(cake_1.price)

cake_3 = Cake()
print(cake_3.coat)
print(cake_3.price)
```


일반적으로 클래스의 속성은 class 문에서 확정하고, 나중에는 수정하지 않는 게 좋다. 자주 변경되어야 하는 속성이 있다면, 그 속성은 클래스가 아니라 인스턴스 속에 정의되어야 할 가능성이 높다.


코드 8-25
```python
class Cake:
   """케이크를 나타내는 클래스"""

   coat = '생크림'

cake_1 = Cake()
cake_2 = Cake()

cake_1.topping = '블루베리'
print(cake_1.topping)

print(Cake.topping)

print(cake_2.topping)
```


코드 8-26
```python
class Cake:
   """케이크를 나타내는 클래스"""

   coat = '생크림'

cake_1 = Cake()
cake_2 = Cake()


print(cake_1.coat)
cake_1.coat = '아이스크림'
print(cake_1.coat)
print(Cake.coat)
print(cake_2.coat)
```


이름공간은 프로그래밍 언어에서 이름이 가리키는 대상을 제한하는 범위이다. 

`dir()` 함수는 파이썬의 기본 데이터 유형이나 다른 사람이 정의한 클래스에 어떤 속성이 있는지 확인할 수 있게 해준다.

코드 8-27
```python
class Cake:
   """케이크를 나타내는 클래스"""

   coat = '생크림'

cake_1 = Cake()
cake_2 = Cake()


import pprint
pprint.pprint(dir(Cake))
```



클래스나 인스턴스에 속한 함수는 그 데이터 종류를 위한 전용. 이 함수를 메서드라 한다.
대부분의 메서드는 클래스 공용 속성으로 정의한다. 데이터 유형을 다루는 방법은 데이터 유형에 따라 정하기 때문이다.


코드 8-29
```python
class Cake:
   """케이크를 나타낸는 클래스"""
   coat = '생크림'

   def describe():
       """이 케이크에 관한 정보를 화면에 출력한다."""
       print('이 케이크는', Cake.coat, '으로 덮여 있다.')

Cake.describe()
```



코드 8-30
```python
class Cake:
   """케이크를 나타낸는 클래스"""
   coat = '생크림'

   def describe():
       """이 케이크에 관한 정보를 화면에 출력한다."""
       print('이 케이크는', Cake.coat, '으로 덮여 있다.')


cake_1 =Cake()
cake_1.coat = '초콜릿'
cake_1.describe()
```

파이썬에서는 클래스를 기준으로 할 때와 인스턴스를 기준으로 할 때 메서드 호출 방식이 다르다. 인스턴스를 기준으로 메서드를 호출하면 암묵적으로 메서드 호출의 기준이 되는 인스턴스가 첫 번째 인자로 메서드에 전달된다.

---
layout: post
title: "연오의 파이썬 공부 10"
date: 2022-08-02 22:15:22 +0900
tags: yeono-python
---

연습문제 7-11

```python
용의자 = [
   {'이름': '멍멍', '털': '흰색', '주둥이': '크다', '발': '크다'},
   {'이름': '킁킁', '털': '검은색', '주둥이': '작다', '발': '크다'},
   {'이름': '왈왈', '털': '흰색', '주둥이': '작다', '발': '크다'},
   {'이름': '꿀꿀', '털': '검은색', '주둥이': '작다', '발': '작다'},
   {'이름': '낑낑', '털': '흰색', '주둥이': '작다', '발': '작다'},
]

# 범인: 주둥이가 작고, 발이 크고, 흰색 털을 가진 개

유력_용의자 = []

for 개 in 용의자:
   if 개['털'] == '흰색' and 개['발'] == '크다' and 개['주둥이'] == '작다':
       유력_용의자.append(개['이름'])

#for 개 in 용의자:
#    용의자.remove(개)

print()
print(유력_용의자)
```

연습문제 7-11 filter 함수 사용해서 작성하기

```python
용의자 = [
   {'이름': '멍멍', '털': '흰색', '주둥이': '크다', '발': '크다'},
   {'이름': '킁킁', '털': '검은색', '주둥이': '작다', '발': '크다'},
   {'이름': '왈왈', '털': '흰색', '주둥이': '작다', '발': '크다'},
   {'이름': '꿀꿀', '털': '검은색', '주둥이': '작다', '발': '작다'},
   {'이름': '낑낑', '털': '흰색', '주둥이': '작다', '발': '크다'},
]

# 범인: 주둥이가 작고, 발이 크고, 흰색 털을 가진 개

주둥이_용의자 = filter(lambda 개 : 개['주둥이'] == '작다', 용의자)
주둥이_발_용의자 = filter(lambda 개: 개['발'] == '크다', 주둥이_용의자)
주둥이_발_털_용의자 = filter(lambda 개: 개['털'] == '흰색', 주둥이_발_용의자)

for 개 in 주둥이_발_털_용의자:
   print(개['이름'])
```

연습문제 7-12

```python
def faulty_rate(diameters):
   # 정상: 지름 0.99mm 이상, 1.01mm 미만

   working_diameters = list(filter(lambda diameter: 0.99 <= diameter < 1.01, diameters))
   return (len(diameters) - len(working_diameters)) / len(diameters)


diameters = [0.985, 0.992, 1.004, 0.995, 0.899, 1.001, 1.002, 1.003, 1.009, 0.998]
print(faulty_rate(diameters))

# 람다식에서 not을 이용하면 더 간단하게 구현할 수 있다.
# lambda diameter: not (0.99 <= diameter < 1.01)
```

연습문제 7-13

```python
fruits = ['배', '사과', '복숭아', '블루베리']

print(sorted(fruits, key=len, reverse=True))
```

#### 반복자와 생성기

반복자: 다음에 무엇을 출력할 차례인지를 기억하여 데이터를 순서대로 꺼낼 수 있도록 돕는 인터페이스이다.
`iter()` 함수는 전달된 데이터의 반복자를 꺼내 반환한다.
`next()` 함수는 반복자를 입력받아 그 반복자가 다음에 출력해야 할 요소를 반환한다.

코드 7-42

```python
it = iter([1, 2, 3])

print(next(it))
print(next(it))
print(next(it))
print(next(it))
```

정해둔 순서대로 값을 생성하는 함수를 정의하면, 이 함수를 이용해 값을 하나씩 꺼내는 반복자를 만들 수 있다. 이런 식으로 값을 생성해 내는 반복자를 생성기라고 한다.

yield 문: return 문처럼 함수가 값을 반환하고 정지하도록 하는데, 그 함수를 나중에 다시 실행하면 정지했던 위치부터 다시 실행되도록 한다.

yield 문을 포함한 함수는 일반적인 함수와 실행 방식이 다르다. 이 함수는 호출되어도 본문을 실행하지 않고 생성기를 반환할 뿐이며, 본문은 생성기가 `next()` 함수에 전달되었을 때 비로소 실행된다.

코드 7-44

```python
def abc():
   """a, b, c를 출력하는 생성기를 반환한다."""
   yield 'a'
   yield 'b'
   yield 'c'

abc_generator = abc()

print(next(abc_generator))
print(next(abc_generator))
print(next(abc_generator))
print(next(abc_generator))
print(next(abc_generator))
```

코드 7-45

```python
def one_to_three():
   """1, 2, 3을 반환하는 생성기를 반환한다."""
   yield 1
   yield 2
   yield 3

one_to_three_generator = one_to_three()

print(next(one_to_three_generator))
print(next(one_to_three_generator))
print(next(one_to_three_generator))
```

코드 7-46

```python
def one_to_infinite():
   """ 1부터 무한대 범위의 자연수를 순서대로 내는 생성기를 반환한다."""

   n = 1
   while True:
       yield n
       n += 1

one_to_infinite_generator = one_to_infinite()

print(next(one_to_infinite_generator))
print(next(one_to_infinite_generator))
print(next(one_to_infinite_generator))
print(next(one_to_infinite_generator))
print(next(one_to_infinite_generator))
print(next(one_to_infinite_generator))
```

생성기는 반복자이므로, 반복자와 마찬가지로 for 문을 순회하거나 리스트, 튜플로 변환할 수 있다.

코드 7-47

```python
def countdown(start, end):
   """start(포함)부터 end(비포함)까지 거꾸로 세는 생성기를 반환한다."""
   n = start
   while end < n:
       yield n
       n -= 1

print(list(countdown(10, 0)))
print(tuple(countdown(100, 95)))
```

생성기 식은 원본 반복 가능 데이터를 가공하여 데이터를 생성한다. 리스트 조건제시법과 유사하다.

코드 7-48

```python
print([e ** 3 for e in range(10)])
```

코드 7-51

```python
cube_generator = (e ** 3 for e in range(100000000))
print(next(cube_generator))
print(next(cube_generator))
print(next(cube_generator))
```

연습문제 7-14

```python
import random

def infinite_random_number(start, end):

   while True:
       yield random.randint(start, end)


infinite_random_number_generator = infinite_random_number(0, 63)
print(next(infinite_random_number_generator))
print(next(infinite_random_number_generator))
print(next(infinite_random_number_generator))
```

연습문제 7-15

```python
def input_orders(n):
   """n개의 음료를 주문받아 리스트로 반환한다."""
   return (input() for _ in range(n))  # return [input() for _ in range(n)]


for drink in input_orders(3):
   print(drink, '만들어 주세요!')

```

## 8장

코드 8-1

```python
coordinate_1 = {'x': 5, 'y': 3}

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
```

코드 8-3

```python
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


coordinate_1 = {'x': 5, 'y': 3}

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

print(circumference_of_triangle(triangle_1))
print(circumference_of_rectangle(rectangle_1))
```

---
layout: post
title: "연오의 파이썬 공부 12"
date: 2022-08-04 22:21:33 +0900
tags: yeono-python
---

코드 8-31

```python
class Cake:
   """케이크를 나타내는 클래스"""
   coat = '생크림'

   def describe(self):
       """이 케이크에 관한 정보를 화면에 출력한다."""
       print('이 케이크는', self.coat, '으로 덮여 있다.')
```

`self`: 파이썬에서 인스턴스를 전달받는 메서드의 매개변수 이름. 관례임.

`Cake()`를 실행해 인스턴스화를 명령하면, 파이썬은 다음 두 단계를 수행한다.

1. `__new__()` 메서드를 실행해 새 객체를 만든다.
2. `__init__()` 메서드를 실행해 객체를 초기화한다.

코드 8-36

```python
class Cake:
   """케이크를 나타낸는 클래스"""
   coat = '생크림'

   def __init__(self, candles):
       """인스턴스를 초기화한다."""
       self.candles = candles

   def describe(self):
       """이 케이크에 관한 정보를 화면에 출력한다."""
       print('이 케이크는', self.coat, '으로 덮여 있다.')
       print('초가', self.candles, '개 꽂혀 있다.')


cake_1 = Cake(12)
cake_2 = Cake(100)

print('케이크 1:')
print('초 개수:', cake_1.candles)

print('케이크 2:')
cake_2.describe()
```

연습문제 8-5

```python
class Coordinate:
   """좌표를 나타내는 클래스"""
   x = 0
   y = 0
```

연습문제 8-6

```python
class Coordinate:
   """좌표를 나타내는 클래스"""
   x = 0
   y = 0


point_1 = Coordinate()
point_1.x = -1
point_1.y = 2

point_2 = Coordinate()
point_2.x = 2
point_2.y = 3
```

연습문제 8-7

```python
import math

class Coordinate:
   """좌표를 나타내는 클래스"""
   x = 0
   y = 0

def square(x):
   """전달받은 수(x)의 제곱을 반환한다."""
   return x * x


def distance(point_1, point_2):
   """두 점 사이의 거리를 계산해 반환한다."""
   return math.sqrt(square(point_1.x - point_2.x) +
                    square(point_1.y - point_2.y))


point_1 = Coordinate()
point_1.x = -1
point_1.y = 2

point_2 = Coordinate()
point_2.x = 2
point_2.y = 3


print(distance(point_1, point_2))
```

연습문제 8-8

```python
import math


def square(x):
   """전달받은 수(x)의 제곱을 반환한다."""
   return x * x


class Coordinate:
   """좌표를 나타낸다."""
   x = 0
   y = 0

   def distance(self, point_2):
       """두 점 사이의 거리를 계산해 반환한다."""
       return math.sqrt(square(self.x - point_2.x) +
                        square(self.y - point_2.y))


point_1 = Coordinate()
point_1.x = -1
point_1.y = 2
point_2 = Coordinate()
point_2.x = 2
point_2.y = 3

print(point_1.distance(point_2))
```

연습문제 8-9

```python

import math


def square(x):
   """전달받은 수(x)의 제곱을 반환한다."""
   return x * x


class Coordinate():
   """좌표를 나타낸다."""

   def __init__(self, x=0, y=0):
       self.x = x
       self.y = y

   def distance(self, point_2):
       """두 점 사이의 거리를 계산해 반환한다."""
       return math.sqrt(square(self.x - point_2.x) +
                        square(self.y - point_2.y))


point_1 = Coordinate(-1, 2)
point_2 = Coordinate(2, 3)

print(point_1.distance(point_2))
```

클래스를 정의할 때 상위 클래스를 지정할 수 있다.

코드 8-41

```python
class Cake:
   """케이크를 나타내는 클래스"""
   coat = '생크림'

   def __init__(self, topping, price, candles=0):
       """인스턴스를 초기화한다."""
       self.topping = topping
       self.price = price
       self.candles = candles

   def describe(self):
       """케이크에 관한 정보를 화면에 출력한다."""
       print('이 케이크는', self.coat, '으로 덮여 있다.')
       print(self.topping, '을 올려 장식했다.')
       print('가격은', self.price, '원이다.')
       print('초가', self.candles, '개 꽂혀 있다.')


class ChocolateCake(Cake):
   coat = '초콜릿'
   cacao_percent = 32.0


print(ChocolateCake.coat)
print(ChocolateCake.cacao_percent)
```

코드 8-42

```python
class Cake:
   """케이크를 나타내는 클래스"""
   coat = '생크림'

   def __init__(self, topping, price, candles=0):
       """인스턴스를 초기화한다."""
       self.topping = topping
       self.price = price
       self.candles = candles

   def describe(self):
       """케이크에 관한 정보를 화면에 출력한다."""
       print('이 케이크는', self.coat, '으로 덮여 있다.')
       print(self.topping, '을 올려 장식했다.')
       print('가격은', self.price, '원이다.')
       print('초가', self.candles, '개 꽂혀 있다.')


class ChocolateCake(Cake):
   coat = '초콜릿'
   cacao_percent = 32.0


print(ChocolateCake.coat)
print(ChocolateCake.cacao_percent)

chocolate_cake_1 = ChocolateCake('막대사탕', 12000)
chocolate_cake_1.describe()
```

코드 8-44

```python
class Cake:
   """케이크를 나타내는 클래스"""
   coat = '생크림'

   def __init__(self, topping, price, candles=0):
       """인스턴스를 초기화한다."""
       self.topping = topping
       self.price = price
       self.candles = candles

   def describe(self):
       """케이크에 관한 정보를 화면에 출력한다."""
       print('이 케이크는', self.coat, '으로 덮여 있다.')
       print(self.topping, '을 올려 장식했다.')
       print('가격은', self.price, '원이다.')
       print('초가', self.candles, '개 꽂혀 있다.')


class ChocolateCake(Cake):
   coat = '초콜릿'
   cacao_percent = 32.0


class IceCreamCake(Cake):
   """아이스크림 케이크를 나타내는 클래스"""

   coat = '아이스크림'
   flavor = '정해지지 않은 맛'

   def __init__(self, flavor, topping, price, candles=0):
       self.flavor = flavor
       super().__init__(topping, price, candles)


ice_cream_cake_1 = IceCreamCake('바닐라맛', '쿠키 인형', 12000)
ice_cream_cake_1.describe()
```

코드 8-46

```python
class Cake:
   """케이크를 나타내는 클래스"""
   coat = '생크림'

   def __init__(self, topping, price, candles=0):
       """인스턴스를 초기화한다."""
       self.topping = topping
       self.price = price
       self.candles = candles

   def describe(self):
       """케이크에 관한 정보를 화면에 출력한다."""
       print('이 케이크는', self.coat, '으로 덮여 있다.')
       print(self.topping, '을 올려 장식했다.')
       print('가격은', self.price, '원이다.')
       print('초가', self.candles, '개 꽂혀 있다.')


class ChocolateCake(Cake):
   coat = '초콜릿'
   cacao_percent = 32.0


class IceCreamCake(Cake):
   """아이스크림 케이크를 나타내는 클래스"""

   coat = '아이스크림'
   flavor = '정해지지 않은 맛'

   def __init__(self, flavor, topping, price, candles=0):
       self.flavor = flavor
       super().__init__(topping, price, candles)


class FruitIceCreamCake(IceCreamCake):
   """과일 아이스크림 케이크를 나타내는 클래스"""
   def __init__(self, fruit_percent, flavor, topping, price, candles=0):
       self.fruit_percent = fruit_percent
       super().__init__(flavor, topping, price, candles)


print(issubclass(ChocolateCake, Cake))
print(issubclass(IceCreamCake, Cake))
print(issubclass(Cake, Cake))
print(issubclass(int, Cake))
```

모든 클래스는 object의 하위클래스이다.

코드 8-48

```python
class Cake:
   """케이크를 나타내는 클래스"""
   coat = '생크림'

   def __init__(self, topping, price, candles=0):
       """인스턴스를 초기화한다."""
       self.topping = topping
       self.price = price
       self.candles = candles

   def describe(self):
       """케이크에 관한 정보를 화면에 출력한다."""
       print('이 케이크는', self.coat, '으로 덮여 있다.')
       print(self.topping, '을 올려 장식했다.')
       print('가격은', self.price, '원이다.')
       print('초가', self.candles, '개 꽂혀 있다.')


class ChocolateCake(Cake):
   coat = '초콜릿'
   cacao_percent = 32.0


class IceCreamCake(Cake):
   """아이스크림 케이크를 나타내는 클래스"""

   coat = '아이스크림'
   flavor = '정해지지 않은 맛'

   def __init__(self, flavor, topping, price, candles=0):
       self.flavor = flavor
       super().__init__(topping, price, candles)


class FruitIceCreamCake(IceCreamCake):
   """과일 아이스크림 케이크를 나타내는 클래스"""
   def __init__(self, fruit_percent, flavor, topping, price, candles=0):
       self.fruit_percent = fruit_percent
       super().__init__(flavor, topping, price, candles)


Cake.radius = 20
print(IceCreamCake.radius)

IceCreamCake.radius = 16
print(Cake.radius)
print(IceCreamCake.radius)

IceCreamCake.temperature = -1
print(FruitIceCreamCake.temperature)
print(Cake.temperature)
```

다중속성이란 클래스가 두 개 이상의 상위 클래스를 나란히 상속하는 것. 수직적 관계로 여러 개의 클래스를 상속하는 것을 다중 상속이라고 하지 않는다.

코드 8-50

```python
class Cake:
   """케이크"""
   coat = '생크림'

class CakePiece:
   """조각케이크"""
   size = 1/ 8
   calorie = 200

class CheeseCake(Cake):
   """치즈케이크"""
   body = '치즈'
   calorie = 1600

class CheeseCakePiece(CakePiece, CheeseCake):
   """치즈 조각 케이크"""
   pass

print('body:', CheeseCakePiece.body)
print('size:', CheeseCakePiece.size)
print('calorie:', CheeseCakePiece.calorie)
```

다중상속한 클래스에서 속성의 이름이 같으면, 왼쪽에 나열한 클래스의 속성부터…

다중상속을 하면 이름공간의 검색 순서가 복잡해져서 인스턴스의 속성이 어느 것을 가르키는지 알기 어려워진다. 그러므로 다중속성은 꼭 필요할 때만 쓰는 것이 바람직하다.

연습문제 8-10

```python
class Shape:
   """도형을 나타내는 클래스"""

   def describe(self):
       """도형의 특징을 화면에 출력한다. 변의 개수는 self.sides 속성을 읽어 구한다."""
       print('이 도형은', self.sides, '개의 변을 가지고 있습니다.')


class Triangle(Shape):
   """삼각형을 나타내는 클래스"""
   sides = 3

class Rectangle(Shape):
   """사각형을 나타내는 클래스"""
   sides = 4


shapes = [
   Triangle(),
   Rectangle(),
   ]
for shape in shapes:
   shape.describe()
```

연습문제 8-11

```python
import math


def square(x):
   """전달받은 수(x)의 제곱을 반환한다."""
   return x * x


class Coordinate():
   """좌표를 나타낸다."""

   def __init__(self, x=0, y=0):
       self.x = x
       self.y = y

   def distance(self, point_2):
       """두 점 사이의 거리를 계산해 반환한다."""
       return math.sqrt(square(self.x - point_2.x) +
                        square(self.y - point_2.y))


class Shape:
   """도형을 나타내는 클래스"""

   def describe(self):
       """도형의 특징을 화면에 출력한다. 변의 개수는 self.sides 속성을 읽어 구한다."""
       print('이 도형은', self.sides, '개의 변을 가지고 있습니다.')


class Triangle(Shape):
   """삼각형을 나타내는 클래스"""
   sides = 3

   def __init__(self, point_a, point_b, point_c):
       self.point_a = point_a
       self.point_b = point_b
       self.point_c = point_c


   def circumference(self):
       """도형의 둘레를 계산하여 반환한다."""
       return self.point_a.distance(self.point_b) + self.point_b.distance(self.point_c) + \
               self.point_c.distance(self.point_a)

class Rectangle(Shape):
   """사각형을 나타내는 클래스"""
   sides = 4

   def __init__(self, point_a, point_b, point_c, point_d):
       self.point_a = point_a
       self.point_b = point_b
       self.point_c = point_c
       self.point_d = point_d


   def circumference(self):
       """도형의 둘레를 계산하여 반환한다."""
       return self.point_a.distance(self.point_b) + self.point_b.distance(self.point_c) + \
               self.point_c.distance(self.point_d) + self.point_d.distance(self.point_a)


shapes = [
   Triangle(Coordinate(0, 0), Coordinate(3, 0), Coordinate(3, 4)),
   Rectangle(Coordinate(2, 2), Coordinate(6, 2), Coordinate(6, 6), Coordinate(2, 6))
]
for shape in shapes:
   shape.describe()
   print('둘레:', shape.circumference())
```

메서드를 활용해 이용해 데이터 유형에 따라 데이터를 다루는 연산을 정의하는 방법을 알아보자.

클래스를 정의할 때 클래스의 속성을 감추어 두고 연산자와 메서드를 인터페이스(한 대상이 다른 대상과 맞닿는 면)로 제공하는 방법을 캡슐화라 한다.

코드 5-52

```python
class FruitJuice():
   """과일 주스"""
   valid_fruits = {'귤', '복숭아', '청포도', '딸기', '사과'}

   def __init__(self):
       """인스턴스를 초기화한다."""
       self.ingredients = []

   def add_ingredient(self, ingredient):
       """재료(ingredient)를 이 주스에 추가한다."""
       if ingredient in self.valid_fruits:
           self.ingredients.append(ingredient)
       else:
           print(ingredient, '는 과일 주스에 넣을 수 없습니다.', sep='')

   def describe(self):
       """이 주스에 관한 정보를 화면에 출력한다."""
       print('이 주스에는', len(self.ingredients), '개의 재료가 들어 있습니다.')
       print('넣은 재료:', end=' ')
       for ingredient in self.ingredients:
           print(ingredient, end=' ')


juice_1 = FruitJuice()
juice_1.add_ingredient('청포도')
juice_1.add_ingredient('복숭아')
juice_1.add_ingredient('도라지')
juice_1.describe()
```

파이썬 문화에서 비공개 속성의 이름은 밑줄 기호 하나로 시작하게 짓는다.

코드 8-54

```python
class FruitJuice():
   """과일 주스"""
   _valid_fruits = {'귤', '복숭아', '청포도', '딸기', '사과'}

   def __init__(self):
       """인스턴스를 초기화한다."""
       self._ingredients = []

   def add_ingredient(self, ingredient):
       """재료(ingredient)를 이 주스에 추가한다."""
       if ingredient in self._valid_fruits:
           self._ingredients.append(ingredient)
       else:
           print(ingredient, '는 과일 주스에 넣을 수 없습니다.', sep='')

   def describe(self):
       """이 주스에 관한 정보를 화면에 출력한다."""
       print('이 주스에는', len(self._ingredients), '개의 재료가 들어 있습니다.')
       print('넣은 재료:', end=' ')
       for ingredient in self._ingredients:
           print(ingredient, end=' ')
```

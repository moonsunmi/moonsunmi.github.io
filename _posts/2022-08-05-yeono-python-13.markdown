---
layout: post
title:  "연오의 파이썬 공부 13"
date:   2022-08-05 22:11:40 +0900
categories: yeono-python
---







코드 8-56
```python
class Food:
   """음식을 나타내는 클래스"""
   def __init__(self, taste, calorie):
       """인스턴스를 초기화한다."""
       self._taste = taste
       self._calorie = calorie

   def to_string(self):
       """음식을 표현하는 문자열을 반환한다"""
       return str(self._taste) + '만큼 맛있고 ' + str(self._calorie) + '만큼 든든한 음식'

   def add(self, other):
       """다른 음식을 더해 새 음식을 반환한다."""
       taste = self._taste + other._taste
       calorie = self._calorie + other._calorie
       return Food(taste, calorie)

food1 = Food(7, 85)
print(food1.to_string())

food2 = Food(12, 266)
print(food2.to_string())

food3 = food1.add(food2)
print(food3.to_string())
```


코드 8-57
```python
# 오류 발생 코드
class Food:
   """음식을 나타내는 클래스"""
   def __init__(self, taste, calorie):
       """인스턴스를 초기화한다."""
       self._taste = taste
       self._calorie = calorie

   def to_string(self):
       """음식을 표현하는 문자열을 반환한다"""
       return str(self._taste) + '만큼 맛있고 ' + str(self._calorie) + '만큼 든든한 음식'

   def add(self, other):
       """다른 음식을 더해 새 음식을 반환한다."""
       taste = self._taste + other._taste
       calorie = self._calorie + other._calorie
       return Food(taste, calorie)


food1 = Food(7, 85)
food2 = Food(12, 266)
print(food1)
print(food1 + food2)
```



문자열 변환, 연산자 실행 등 특정한 경우에 저절로 실행되는 메서드를 이중 밑줄 메서드라 한다. 
연습문제 8-12
```python
import random
class Dice:
   """다면체 주사위"""
   def __init__(self, sides):
       self._sides = sides
       self._face = random.randint(1, sides)  # <<== 잘못된 곳

   def top(self):
       """현재 나온 면을 반환한다."""
       return self._face

   def roll(self):
       """나온 면을 새 임의 값으로 설정하고 반환한다."""
       self._face = random.randint(1, self._sides)
       return self._face

dice_4 = Dice(4) # 사면체 주사위 생성
print('사면체 주사위 테스트 ----')
print('처음 나온 면:', dice_4.top())
print('다시 굴리기:', dice_4.roll())
print('다시 굴리기:', dice_4.roll())

dice_100 = Dice(100) # 백면체 주사위 생성
print('백면체 주사위 테스트 ----')
print('처음 나온 면:', dice_100.top())
print('다시 굴리기:', dice_100.roll())
print('다시 굴리기:', dice_100.roll())


# 모범답안에서는 _face를 초기화할 때 roll 메서드를 활용했음.
```



연습문제 8-13
```python
class Food:
   """음식을 나타내는 클래스"""
   def __init__(self, taste, calorie):
       """인스턴스를 초기화한다."""
       self._taste = taste
       self._calorie = calorie

   def __str__(self):
       """음식을 표현하는 문자열을 반환한다"""
       return str(self._taste) + '만큼 맛있고 ' + str(self._calorie) + '만큼 든든한 음식'

   def __add__(self, other):
       """다른 음식을 더해 새 음식을 반환한다."""
       taste = self._taste + other._taste
       calorie = self._calorie + other._calorie
       return Food(taste, calorie)

   """ 맛이 좋으면 더 큰것, 같은 맛일 때는 칼로리가 적은 게 큰 것"""
   def __lt__(self, other):  # self < other
       if self._taste < other._taste:
           return True
       elif self._taste == other._taste:
           if self._calorie > other._calorie:
               return True
       return False

   def __gt__(self, other):  # self > other
       if self._taste > other._taste:
           return True
       elif self._taste == other._taste:
           if self._calorie < other._calorie:
               return True
       return False

   def __le__(self, other):  # self <= other
       if self.__lt__(other) or self.__eq__(other):
           return True
       else:
           return False

   def __ge__(self, other):  # self >= other
       if self.__gt__(other) or self.__eq__(other):
           return True
       else:
           return False

   def __eq__(self, other):  # self == other
       return self._taste == other._taste and self._calorie == other._calorie

   def __ne__(self, other):  # self != other
       return self._taste != other._taste or self._calorie != other._calorie


strawberry = Food(9, 32)
potato = Food(6, 66)
sweet_potato = Food(12, 131)
pizza = Food(13, 266)
print('딸기 < 감자: ', strawberry < potato)
print('감자 + 감자 < 고구마: ', potato + potato < sweet_potato)
print('피자 >= 딸기: ', pizza >= strawberry)
print('피자 >= 피자: ', pizza >= pizza)
print('감자 + 딸기 < 피자: ', potato + strawberry < pizza)
print('딸기 == 딸기: ', potato == potato)
```



연습문제 8-13 모범 답안에 가깝게 upgrade
```python
# 연습문제 8-13을 모범답안에 가깝게 수정한 것
# Food 메서드를 구현할 때, 사용할 수 있는 메서드를 좀 더 이용함.
class Food:
   """음식을 나타내는 클래스"""
   def __init__(self, taste, calorie):
       """인스턴스를 초기화한다."""
       self._taste = taste
       self._calorie = calorie

   def __str__(self):
       """음식을 표현하는 문자열을 반환한다"""
       return str(self._taste) + '만큼 맛있고 ' + str(self._calorie) + '만큼 든든한 음식'

   def __add__(self, other):
       """다른 음식을 더해 새 음식을 반환한다."""
       taste = self._taste + other._taste
       calorie = self._calorie + other._calorie
       return Food(taste, calorie)

   """ 맛이 좋으면 더 큰것, 같은 맛일 때는 칼로리가 적은 게 큰 것"""

   def __eq__(self, other):  # self == other
       return self._taste == other._taste and self._calorie == other._calorie

   def __lt__(self, other):  # self < other
       if self._taste < other._taste:
           return True
       elif self._taste == other._taste:
           if self._calorie > other._calorie:
               return True
       return False

   def __gt__(self, other):  # self > other
       return other < self

   def __le__(self, other):  # self <= other
       return self < other or self == other

   def __ge__(self, other):  # self >= other
       return other <= self

   def __ne__(self, other):  # self != other
       return not self == other


strawberry = Food(9, 32)
potato = Food(6, 66)
sweet_potato = Food(12, 131)
pizza = Food(13, 266)
print('딸기 < 감자: ', strawberry < potato)
print('감자 + 감자 < 고구마: ', potato + potato < sweet_potato)
print('피자 >= 딸기: ', pizza >= strawberry)
print('피자 >= 피자: ', pizza >= pizza)
print('감자 + 딸기 < 피자: ', potato + strawberry < pizza)
print('딸기 == 딸기: ', potato == potato)

# 메서드를 만든 순간부터, 다른 메서드에서도 활용할 수 있을지 계속 생각해야 겠다.
```


연습문제 8-14

파이썬 인터프리터가 막는다.



## 9장



* 구문오류: 프로그램이 문법적으로 잘못됐다. 
* 실행시간 오류: 프로그램의 지시를 실행할 수 없다. 
* 논리 오류: 프로그램이 논리적으로 잘못됐다.



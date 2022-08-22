---
layout: post
title:  "연오의 파이썬 공부 3"
date:   2022-07-26 20:50:01 +0900
categories: yeono-python
---



연습문제 3-4
```python
def print_plus(num1, num2):
   """두 수(num1, num2)를 전달받고, 그 합계를 출력한다."""
   print(num1 + num2)
  

print_plus(100, 50)
```



연습문제 3-5
```python
def average_of_4_numbers(number1, number2, number3, number4):
   """네 수(number1, number2, number3, number4)를 전달 받아 평균을 반환한다."""
   total = number1 + number2 + number3 + number4
   return total / 4


print(average_of_4_numbers(512, 64, 256, 192))
```



연습문제 3-6
```python
def no_return():
   """화면에 메시지를 출력하지만, 값을 반환하지는 않는다."""
   print('이 함수에는 반환값이 없습니다.')


result = no_return()
print(result)
```


연습문제 3-7
```python
def area_of_triangle(base, height):
   """밑변(base)과 높이(height)를 전달 받아, 삼각형의 넓이를 반환한다."""
   return base * height / 2


print(area_of_triangle(10, 8))
```



### 3.4 전역변수와 지역변수

문맥에 따라 다른 데이터를 가리키는 같은 이름이 생길 수 있다.
프로그래밍에서는 이름공간(namespace)이라는 개념을 이용해 이름의 문맥을 구별한다. 이름공간은 변수의 이름을 정의해둔 공간이다.

* 전역 이름공간: 프로그램 전체 범위의 이름을 담는다. -> 전역변수
* 지역 이름공간: 한정적인 문맥의 이름을 담는다. -> 지역변수

모든 함수는 자신만의 지역 이름공간을 가지며, 함수 속에서 작성한 변수는 그 함수의 지역변수가 된다. 함수의 지역변수는 함수가 실행되는 동안에만 존재한다. 따라서 함수의 이전 실행 결과를 기억해야 한다면 함수의 밖에서 결과를 보관해 주어야 한다.

전역변수는 어디에서나 읽을 수 있지만, 함수 안에서 전역변수에 새로운 값을 대입하는 것은 금지된다.(global 문을 사용하면 예외적으로 가능하다)


전역변수와 지역변수를 구별해서 사용하는 이유
함수는 문제를 작은 문제로 나누어 해결하기 위해서 사용한다. 문제를 나누어 해결하려면, 다른 문제를 배제하고 지금 다루는 문제만을 생각할 수 있어야 한다. 자기 문제에 필요한 데이터만 조작해야 한다.


다음 코드에서 오류가 나는 이유
코드 3-12
```python
num_stamp = 0

def stamp():
   """쿠폰 ~~"""
   num_stamp = num_stamp + 1
   print(num_stamp)

stamp()
```
함수에서는 전역변수에 값을 할당할 수 없으므로, num_stamp에 값을 대입할 때 num_stamp를 지역변수로 해석한다. 따라서 지역 변수에 값을 대입하기 전에 참조했다는 오류가 생긴다.


다음은 global 문으로 전역변수를 수정 가능하게 한 것.

코드 3-13
```python
num_stamp = 0

def stamp():
   """쿠폰 ~~"""
   global num_stamp
   num_stamp = num_stamp + 1
   print(num_stamp)
  
stamp()
stamp()
```

가능하면 전역 변수를 수정하지 말고, 다음처럼 매개변수와 반환값을 이용하자.

코드 3-14
```python
num_stamp = 0

def stamp(num_stamp):
   """쿠폰 스탬프가 찍힌 횟수를 증가시키고, 화면에 출력한다."""

   num_stamp = num_stamp + 1
   print(num_stamp)
   return num_stamp

num_stamp = stamp(num_stamp)
num_stamp = stamp(num_stamp)
```


연습문제 3-8

나의 답안
전역변수: pi, result (=> 정답: pi, result, area_of_circle, volume_of_cylinder, print)
area_of_circle() 함수의 지역 변수: radius, area
volume_of_cylinder() 함수의 지역 변수: radius, height, top_area, volume


* 오답노트: 
파이썬에서는 함수의 이름도 전역변수에 포함된다.
함수를 "정의할 때" 변수에 값(함수)이 대입되기 때문입니다.

```python
def foo(x): return x + 1
```
와 같이 함수를 def 문으로 정의했을 때, (x 를 인자로 받아서 x + 1 을 반환하는 함수 객체)가 생성되고, foo 이라는 변수에 이 함수 객체가 대입되는 것입니다.


함수를 정의할 때 매개변수에 인자를 전달하지 않을 경우 사용할 기본값을 정의해 둘 수 있다.




이때, 위치에 주의! 기본값을 정의해둔 매개변수가 있다면, 그다음에 나오는 매개변수들에도 꼭 기본값이 지정되어야 한다. (순서를 정해 두지 않는다면, 생략된 인자가 어느 매개변수에 전달될지 모르기 때문)





함수를 호출할 때 전달받을 매개변수를 지정할 수 있다.


파이참에서는 매개변수를 팝업으로 보여준다.





#### print() 함수의 선택적 매개변수 사용하기

print() 함수를 실행하면 매개변수에 전달한 인자가 출력되고 행이 바뀐다. 다음과 같이 마지막에 출력하는 매개변수 end의 값이 개행으로 되어 있기 때문에






연습문제 3-9
```python
print('주문하실 음료: ', end='')
drink = input()
print('주문하실 간식: ', end='')
snack = input()

print(drink, snack, '주문 받았습니다.')
```



---
layout: post
title: "연오의 파이썬 공부 7"
date: 2022-07-30 22:22:12 +0900
tags: yeono-python
---

### 패킹과 언패킹

패킹: 여러 개의 데이터를 컬렉션으로 묶어 변수에 대입하는 것
변수 하나가 여러 개의 데이터를 가리키도록 -> 컬렉션에 담고 그 컬렉션을 변수에 대입할 수 있다.

컬렉션의 요소를 여러 개의 변수에 나누어 담는 방법을 언패킹이라 한다.
시퀀스에 시퀀스를 대입하면, 각 요소가 순서대로 대입된다.

튜플은 내용을 변경할 수 없지 않나!라는 생각이 잠깐 들었지만… a, b, c, d, e들은 튜플 안의 요소로 불변이 아니라서 괜찮은듯
각 시퀀스의 사이즈가 같아야, 빈칸은 `_`로

대입문에서 좌변의 변수 중 하나에 별 기호를 붙이면, 남은 요소 전체를 리스트에 담아 대입한다.

대입문의 양변에 요소 튜플과 데이터의 튜플을 나열하면 여러 행의 대입문을 한 행으로 줄일 수 있다.

시퀀스 앞에 별 기호를 붙이면 시퀀스를 풀어서 전달할 수 있다.

함수에서 임의의 개수만큼 데이터 전달받기: 여러 개의 데이터를 시퀀스로 묶어 하나의 매개변수에 전달받을 수 있다.

할인율 매개변수는 필수, 그 외의 데이터는 전부 구매가\_목록 매개변수로

### 매핑(함수)을 매개변수에 전달하기

매킹의 키(y,m, d)와 함수의 매개변수 (y,m,d) 이름이 같다. 이럴 때 데이터를 풀어 이름이 같은 짝에 전달되도록 할 수 있다. 매핑은 \*\*로 언패킹할 수 있다.

매핑의 키와 함수의 매개변수 이름이 다르면 오류가 발생한다.

### 키 인자들 kwargs 사용하기

매개변수를 매핑으로 묶어서 전달하는 방법은 함수에서 처리해야 할 매개변수의 종류가 너무 많아 목록에 다 정의하기 힘들거나 필수 매개변수와 구분되는 선택적 매개변수를 정의할 때 자주 활용된다.

연습문제 5-18

```python
# 변수에 대입되어 있는 데이터
x = 10
y = -20

#두 변수의 데이터를 서로 교환하기
temp = x
x = y
y = temp

print(x)
print(y)

# 패킹의 연습문제였는데... 패킹을 쓸 생각을 못 했다.
# 모범 답안: x, y = y, x
# 대입 연산자 우변의 (y, x) 튜플이 먼저 (10, -20) 튜플로 평가되며, 그 다음으로 좌변의 변수 x와 변수 y에 우변의 튜플의 원소가 각각 대입된다.
```

연습문제 5-19

```python
def gap(*args):
   """여러 개의 수를 전달받아, 인자 중 가장 큰 수와 가장 작은 수의 차이를 반환한다."""

   return max(args) - min(args)


print(gap(100))
print(gap(10, 20, 30, 40))
ages = [19, 16, 24, 19, 23]
print(gap(*ages))
```

연습문제 5-20

```python
def concatenate(*text, seperator=''):
   """여러 개의 문자열을 연결해 반환한다"""
   # text = str(text)
   result = seperator.join(text)
   print(result)


concatenate('가난하다고', '해서', '외로움을', '모르겠는가', seperator='/')
concatenate(*'월화수목금토일', seperator=' - ')


# 매개변수 text의 타입이 튜플임. join을 쓰려면 문자열이어야겠다는 생각에 문자열로 바꿨더니, 모든 글자마다 seperator가 들어갔음..

```

`“-”,join([“a”, “b”, “c”])`
=> `“a-b-c”`

## 6장 선택과 반복으로 실행 흐름 조정하기

if_1.py

```python
print('일요일 낮의 날씨를 입력해 주세요.:')
날씨 = input()

if 날씨 == '비':
   print('집에서 프로그램을 만들자.')

if 날씨 != '비':
   print('공원에서 스케이트보드를 타자')
```

absolute.py

```python
def 절댓값(n):
   """수 n을 입력받아, 절댓값을 반환한다."""
   if n >= 0:
       return n
   if n < 0:
       return -n


print('10의 절댓값:', 절댓값(10))
print('-5의 절댓값:', 절댓값(-5))
```

if_2.py

```python
print('일요일 낮의 날씨를 입력해 주세요.')
날씨 = input()

if 날씨 == '비':
   print('집에서 프로그램을 만들자.')
else:
   print('공원에서 스케이트보드를 타자.')

```

`else`를 이용해 두 번째 조건을 대체할 수 있는 경우는.. 한 조건이 참일 때, 다른 조건이 거짓이 되는 모순에서만 가능하다.

빈 코드 블록 정의할 때, pass 문

코드 6-5

```python
print('일요일 낮의 날씨를 입력해 주세요.')

날씨 = input()
if 날씨 == '비':
   pass
else:
   print('공원에서 스케이트를 타자.')
```

코드 6-6

```python
print('일요일 낮의 날씨를 입력해 주세요:')
날씨 = input()

if not (날씨 == '비'):
   print('공원에서 스케이트보드를 타자.')
```

`not (날씨 == ‘비)` 혹은 `날씨 != 비`

선택지가 여러 개일 때 elif
elif 절은 자신의 앞에 나오는 조건이 거짓일 때만 자기 조건을 검사한다.

if_3.py

```python
print('일요일 낮의 기온을 입력해 주세요:')
기온 = float(input())

if 28.0 <= 기온:
   print('바닷가에서 더위를 피하자.')
elif 16.0 <= 기온:
   print('공원에서 스케이트보드를 타자.')
elif 8.0 <= 기온:
   print('도서관에서 책을 읽자.')
else:
   print('집에서 프로그램을 만들자.')
```

if_4.py

```python
print('일요일 낮의 기온을 입력해 주세요.:')
기온 = float(input())

if 28.0 <= 기온:
   print('바닷가에서 더위를 피하자.')

if 16.0 <= 기온:
   print('공원에서 스케이트보드를 타자.')

if 8.0 <= 기온:
   print('도서관에서 책을 읽자.')
else:
   print('집에서 프로그램을 만들자.')
```

여러 조건을 결합하고 싶다면 and, or 연산 등을 이용한다.

계절이 봄 또는 가을이다라는 조건을 나타낼 때
`계절 == ‘봄' or ‘가을'` x
`계절 == ‘봄' or 계절 == ‘가을'` o

조건부 식은 조건에 따라 값을 구하는 식이다.

if문으로도 비슷한 작업을 수행할 수 있지만, if 문은 식이 아니라 명령문이기 때문에 평가 결과를 직접 변수에 대입하는 것이 불가능하다.

if A and B 조건문은 다음과 같이 변경 가능

```python
if A:
    if B:
        ;
```

if A or B 조건문은 다음과 같이 변경 가능

```python
if A:
  ;
else:
    if B
        ;
```

연습문제 6-1

```python
def price(number):
   """쇼핑몰에서 구매할 상품 개수(number)를 입력받아, 총 지불해야 할 가격을 계산한다."""

   if number < 10:
       return number * 100
   elif number < 30:
       return number * 95
   elif number < 100:
       return number * 90
   else:
       return number * 85


print(price(8))
print(price(12))
print(price(38))
print(price(1000))
```

연습문제 6-2

```python
def is_leap_year(this_year):
   """연도(this_year)를 입력받아 그해가 윤년이면 True를 아니면 False를 반환한다."""

   if this_year % 4 == 0:
       if this_year % 400 == 0:
           return True
       elif this_year % 100 == 0:
           return False
       else:
           return True
   else:
       return False


print('1996년은 윤년인가?', is_leap_year(1996))
print('1900년은 윤년인가?', is_leap_year(1900))
print('2000년은 윤년인가?', is_leap_year(2000))
print('2020년은 윤년인가?', is_leap_year(2020))
print('2021년은 윤년인가?', is_leap_year(2021))


# 윤년 조건에 '100으로 나누어 떨어지면 윤년이 아니다.' '400으로 나누어 떨어지면 윤년이다.'라는 조건이 있다.
# 100을 먼저 물어 보면, 400에서 걸러지기 때문에 400을 먼저 물어보는 식으로 작성했는데... 썩 좋은 방법은 아니었던 것 같다.
# and 연산일 때 if 중첩으로...
```

연습문제 6-2\_모범답안 방법대로 수정한 버전

```python
def is_leap_year(this_year):
   """연도(this_year)를 입력받아 그해가 윤년이면 True를 아니면 False를 반환한다."""

   if this_year % 4 == 0:
       if this_year % 100 == 0:
           if this_year % 400 == 0:
               return True
           else:
               return False
       else:
           return True
   else:
       return False


print('1996년은 윤년인가?', is_leap_year(1996))
print('1900년은 윤년인가?', is_leap_year(1900))
print('2000년은 윤년인가?', is_leap_year(2000))
print('2020년은 윤년인가?', is_leap_year(2020))
print('2021년은 윤년인가?', is_leap_year(2021))


# 윤년 조건에 '100으로 나누어 떨어지면 윤년이 아니다.' '400으로 나누어 떨어지면 윤년이다.'라는 조건이 있다.
# 100을 먼저 물어 보면, 400에서 걸러지기 때문에 400을 먼저 물어보는 식으로 작성했는데... 썩 좋은 방법은 아니었던 것 같다.
# and 연산일 때 if 중첩으로...

```

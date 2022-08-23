---
layout: post
title:  "연오의 파이썬 공부 8"
date:   2022-07-31 22:23:42 +0900
categories: yeono-python
---





```python
days = {
    1: lambda(year): 31,
    2: lambda(year) 29 if is_leap_year(year) else 28,
    3: lambda(year) 31, …
}

days = {
    1: 31,
    ‘2_on_leap_year’: 29,
    2: 28,
}

days[1](2020)
days[2](2020)
```

연습문제 6-3
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


def days_in_month(year, month):
   """연도(year)와 월(month)을 입력받아, 그달이 며칠까지 있는지 반환한다."""

   number_of_days_per_month = {1: 31, 3: 31, 4: 30, 5: 31, 6: 30, 7: 31, 8: 31, 9: 30, 10: 31, 11: 30, 12: 31}
   if is_leap_year(year):
       number_of_days_feb = 29
   else:
       number_of_days_feb = 28
   number_of_days_per_month.update({2: number_of_days_feb})

   return number_of_days_per_month.get(month)


print(days_in_month(2022, 2))
print(days_in_month(2000, 10))
print(days_in_month(1996, 2))
print(days_in_month(1998, 1))
```



연습문제 6-3 모범 답안 방식대로 업데이트

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


def days_in_month(year, month):
   """연도(year)와 월(month)을 입력받아, 그달이 며칠까지 있는지 반환한다."""

   month_have_31_days = {1, 3, 5, 7, 8, 10, 12}
   month_have_30_days = {4, 6, 9, 11}
   month_have_28_or_29_days = {2}


   if month in month_have_31_days:
       return 31
   elif month in month_have_30_days:
       return 30
   else:
      if is_leap_year(year):
          return 29
      else:
          return 28


print('2020년 2월의 날의 수:', days_in_month(2020, 2))
print('2021년 2월의 날의 수:', days_in_month(2021, 2))
print('2021년 5월의 날의 수:', days_in_month(2020, 5))
print('2021년 9월의 날의 수:', days_in_month(2020, 11))


# 모범 답안 방식대로 수정한 버전
```





연습문제 6-4

```python
"""
키와 몸무게를 입력 받아 비만 정도를 알려준다.

비만도 측정하는 방법:
1. 체질량 지수 = w(체중) / {t(키) * t(키)}
2. 체질량 지수를 이용한 비만 정도 계산법
   체질량 지수 < 18.5   → 저체중
   18.5 <= 체질량 지수 < 23    → 정상
   23 <= 체질량 지수 < 25   → 과체중
   체질량 지수 >= 25     → 비만
"""


def calculate_BMI(t, w):
   return w / (t * t)


def degree_of_obesity(BMI):
   if BMI < 18.5:
       print('저체중입니다.')
   elif BMI < 23:
       print('정상입니다.')
   elif BMI < 25:
       print('과체중입니다.')
   else:
       print('비만입니다.')


print('키를 입력하세요(m): ', end='')
t = float(input())
print('몸무게를 입력하세요(kg): ', end='')
w = float(input())

degree_of_obesity(calculate_BMI(t, w))
```




연습문제 6-5

```python
def absolute_number(number):
   """수(number)의 절댓값을 구한다. 조건부 식을 활용해서 작성되었음."""

   number = number * -1 if number < 0 else number

   return number


print(absolute_number(-2))
print(absolute_number(3))
print(absolute_number(0))

```




반복이란 같은 작업을 여러 번 실행하는 것

while 문은 지정한 조건이 유지되는 동안 코드를 계속 반복하는 명령이다.

코드 6-18
```python
rainbow = ['Red', 'Orange', 'Yellow', 'Green', 'Blue', 'Indigo', 'Violet']
index = 0

while index < len(rainbow):
   print(rainbow[index])
   index += 1
```



코드 6-19
```python
text = "아무 메시지나 입력하세요. 그만하려면 '그만'을 입력하세요."
while text != '그만':
   print('컴퓨터: ' + text)
   text = input()


for 문도 반복 실행하는 명령어.

while 문이 여러 목적에 활용할 수 있는 범용적인 반복 기능이라면, for 문은 순차적 처리와 컬렉션 순회에 특화된 반복 기능이다. for 문을 이용하면 반복 횟수를 관리하는 데 신경을 쓰지 않고 간단히 컬렉션을 순회할 수 있다.


코드 6-21

rainbow = ['Red', 'Orange', 'Yellow', 'Green', 'Blue', 'Indigo', 'Violet']

for color in rainbow:
   print(color)
```





코드 6-22
```python
def my_sum(numbers):
   """numbers의 모든 요소의 합을 반환한다."""

   total = 0
   for n in numbers:
       total += n
   return total

print(my_sum([1, 2, 3, 4, 5]))
```



코드 6-23
```python
total = 0

for n in [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]:
   if n % 2 == 0:
       total += n
      
print(total)
```


1부터 1천만까지의 모든 자연수 같은 걸 range로 쉽게 구할 수 있다.

코드 6-24
```python
total = 0

for n in range(1, 10_000_001):
   if n % 2 == 0:
       total += n

print(total)
```




짝수의 합 구하는 것 range로 깔끔하게
코드 6-25
```python
total = 0

for n in range(2, 10_000_0001, 2):
   total += n

print(total)
```



컬렉션의 각 요소가 무엇인지는 의미가 없으면, 요소를 대입받을 변수의 이름을 _로 지정.
코드 6-26
```python
print('몇 번 반복하시겠습니까?')
n = int(input())

for _ in range(n):
   print('안녕')
```

for 문은 컬렉션을 순서대로 순회할 때 편리하게 사용할 수 있다. while 문을 사용하면 반복 과정을 제어하는 코드를 직접 작성해야 해 불편하다.





* continue 문과 break 문으로 실행 흐름을 중지할 수 있다.
* continue 문: 이번 주기의 실행을 중지
* break 문: 반복 전체를 중지


코드 6-27
```python
for i in range(4):
   print('현재 반복 주기:', i)
   continue
   print('다음 반복 주기:', i + 1)
```


코드 6-28
```python
for i in range(4):
   print('현재 반복 주기:', i)
   break
   print('다음 반복 주기:', i + 1)
```



break와 continue의 차이를 보여주는 넘 좋은 예제
코드 6-29
```python
total = 0

while True:
   print('더할 수를 입력하세요: ', end='')
   user_input = input()

   if user_input == '그만':
       break

   if not user_input.isnumeric():
       print('잘못된 입력입니다.')
       continue

   total += int(user_input)
   print('합계:', total)

print('프로그램을 종료합니다.')


while else절

코드 6-30
i = 0
while(i<3):
   print(i, ' 번째 실행')
   i += 1
else:
   print('반복완료')
```





코드 6-31
```python
i = 0
while(i < 100):
   print(i, '번째 실행')
   i += 1
   if (i > 2):
       print('반복 중지')
       break
else:
   print('반복 완료')
```





코드 6-32
```python
for i in range(3):
   print(i, '번째 실행')
else:
   print('반복 완료')

```


while 문의 else 절은 조건이 거짓일 때 실행, for 문의 else 절은 정상 종료된 직후 실행.
단순히 반복이 끝난 후에 코드를 실행해야 한다면 그냥 다음 행에 코드를 입력하면 된다. else 절의 가치는 반복이 정상적으로 종료되었는지 아니면 임의로 중지되었는지를 구분하는 데 있다.



코드 6-33
```python
def 첫_짝수_찾기(numbers):
   """numbers에서 첫 번째 짝수를 찾아 화면에 출력한다."""
   for n in numbers:
       if n % 2 == 0:
           print(n, '이 첫 짝수입니다.')
           break
   else:
       print('짝수가 없습니다.')

첫_짝수_찾기([1, 3, 5, 33, 47, 55])
첫_짝수_찾기([7, 5, 6, 72, 19, 81])
```



코드 6-34
```python
for i in range(2, 10):
   for j in range(1, 10):
       print(i * j, end= ' ')
   print()
```



연습문제 6-6
```python
"""100 이상, 10,000 미만인 자연수 중에서 5의 배수를 모두 합하면 얼마인지 계산하라."""

n = 100
result = 0

while(n < 10_000):
   result += n
   n += 5

print(result)
```




연습문제 6-7
```python
"""
100 이상, 10,000 미만인 자연수 중에서 5의 배수를 모두 합하면 얼마인지 계산하라.
for, range version
"""

result = 0
for n in range(100, 10_000, 5):
   result += n

print(result)
```



연습문제 6-8
```python
def max_in_list(numbers):
   """" 리스트 하나를 매개변수로 전달받아, 리스트에서 가장 큰 요소를 반환한다."""

   max_value = float('-inf')

   for number in numbers:
       if number > max_value:
           max_value = number

   return max_value


print(max_in_list([1, 2, 3]))
print(max_in_list([666, 33, -30, 222]))
```



연습문제 6-9
```python
def is_prime(number):
   """어떤 수(number)를 입력 받아 그 수가 소수인지를 반환한다. (소수면 True, 아니면 False)"""

   if number == 1:
       return False

   for i in range(2, number):
       if number % i == 0:
           return False

   return True


sum = 0
for i in range(1, 100):
   if is_prime(i):
       sum += i

print(sum)

```





단축 평가란 계산을 진행하는 도중에 결과가 이미 확정되었다면 나머지 계산 과정을 생략하는 것이다.


코드 6-37
```python
def 홀짝(n):
   """n이 홀수인지 짝수인지를 화면에 출력한다."""
   (n % 2 == 0) and print(n, '은 짝수입니다.')
   (n % 2 == 0) or print(n, '은 홀수입니다.')

홀짝(13)
홀짝(10)
```


재귀란 함수가 자신을 직접 또는 간접적으로 호출하는 것을 말한다. while 문이나 for 문 없이도 반복 작업을 수행할 수 있다.


코드 6-38
```python
def 자연수(n, m):
   """수 n을 출력하고, 1 더한 수가 m보다 작으면 그 수도 출력한다."""
   print(n)
   if n + 1 < m:
       자연수(n + 1, m)

자연수(4, 8)
```




코드 6-39
```python
def n번째_피보나치_수(n):
   """n번째 피보나치 수를 반환한다."""

   if n == 1:
       return 1
   elif n == 2:
       return 1
   else:
       return n번째_피보나치_수(n-1) + n번째_피보나치_수(n-2)

for n in range(1, 12):
   print(n번째_피보나치_수(n), end = ' ')
```


재귀로 작성한 코드는 동일한 작업을 수행하는 while 문으로 수정할 수 있다.





## 7장



컬렉션마다 담을 수 있는 데이터의 종류가 다르다. 그런데 리스트와 사전은 그 속에 담는 데이터의 유형을 가리지 않는다.


데이터를 어떻게 구조화하는가에 따라 데이터를 사용하는 방법과 프로그램을 작성하는 방법도 달라진다.







데이터를 좀 더 쉽게 알아보고 싶다면 `pprint()` 함수를 사용한다. `pprint()` 함수를 이용하면 컬렉션의 요소를 적절히 개행하고 들여쓰기하여 보기 좋게 출력된다.


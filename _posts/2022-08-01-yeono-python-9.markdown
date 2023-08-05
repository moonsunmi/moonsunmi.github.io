---
layout: post
title: "연오의 파이썬 공부 9"
date: 2022-08-01 22:35:12 +0900
tags: yeono-python
---

연습문제 7-1

```python
weather_of_9_1 = [
   {'경기', '맑음', 27.2, 0.4, 0.1},
   {'강원', '맑음', 23.6, 0.6, 0.1},
   {'충청', '맑음', 24.4, 0.35, 0.1},
   {'경상', '맑음', 26, 0.55, 0.1},
   {'전라', '맑음', 27, 0.4, 0},
   {'제주', '구름 조금', 26.4, 0.45, 0.1},
]
```

연습문제 7-2

```python
weather_of_9_1 = [
   {'지역': '경기', '날씨': '맑음', '기온': 27.2, '습도': 0.4, '강수 확률': 0.1},
   {'지역': '강원', '날씨': '맑음', '기온': 23.6, '습도': 0.6, '강수 확률': 0.1},
   {'지역': '충청', '날씨': '맑음', '기온': 24.4, '습도': 0.35, '강수 확률': 0.1},
   {'지역': '경상', '날씨': '맑음', '기온': 26, '습도': 0.55, '강수 확률': 0.1},
   {'지역': '전라', '날씨': '맑음', '기온': 27, '습도': 0.4, '강수 확률': 0},
   {'지역': '제주', '날씨': '구름 조금', '기온': 26.4, '습도': 0.45, '강수 확률': 0.1},
]
```

연습문제 7-3

```python
import pprint

weather_of_9_1 = [
   {'지역': '경기', '날씨': '맑음', '기온': 27.2, '습도': 0.4, '강수 확률': 0.1},
   {'지역': '강원', '날씨': '맑음', '기온': 23.6, '습도': 0.6, '강수 확률': 0.1},
   {'지역': '충청', '날씨': '맑음', '기온': 24.4, '습도': 0.35, '강수 확률': 0.1},
   {'지역': '경상', '날씨': '맑음', '기온': 26, '습도': 0.55, '강수 확률': 0.1},
   {'지역': '전라', '날씨': '맑음', '기온': 27, '습도': 0.4, '강수 확률': 0},
   {'지역': '제주', '날씨': '구름 조금', '기온': 26.4, '습도': 0.45, '강수 확률': 0.1},
]

pprint.pprint(weather_of_9_1)
```

`pprint.pprint()` 함수의 출력 스타일은 한 행에 전부 출력하되, 한 행에 전부 출력할 수 없을 때만 요소들을 개행하여 출력하는 방식이다.

시퀀스 for 문으로 순회했다. 문자열도 시퀀스이기 때문에 순회가 가능하다.

집합도 순회가 가능하나 어떤 순서로 원소가 대입될지는 알 수 없다.

for 문에 매핑을 그냥 입력하면 키를 순회한다.

코드 7-12

```python
drinks = {
   '아메리카노': 2500,
   '카페라테': 3000,
   '딸기 주스': 3500,
}

for k in drinks:
   print(k)
   print(drinks[k])
```

사전의 키-값 쌍의 시퀀스를 구하는 `item()` 메서드

코드 7-13

```python
drinks = {
   '아메리카노': 2500,
   '카페라테': 3000,
   '딸기 주스': 3500,
}

for k, v in drinks.items():
   print(k)
   print(v)
```

코드 7-16

```python
board = [
   [['black', '룩'], None, None, None, None, None, None, None],
   [None, None, None, ['black', '킹'], None, None, None, None],
   [None, None, None, None, None, None, None, None],
   [None, None, None, None, None, None, None, None],
   [None, None, ['white', '비숍'], None, None, None, None, None],
   [None, None, None, None, None, None, None, None],
   [None, None, None, None, None, None, None, None],
   [None, None, None, None, ['white', '킹'], None, None, None],
]

for row in board:
   for piece in row:
       if piece:
           print('I', end=' ')
       else:
           print('.', end = ' ')
   print()
```

코드 7-16 체스말에 색깔만 구별한 코드

```python
board = [
   [['black', '룩'], None, None, None, None, None, None, None],
   [None, None, None, ['black', '킹'], None, None, None, None],
   [None, None, None, None, None, None, None, None],
   [None, None, None, None, None, None, None, None],
   [None, None, ['white', '비숍'], None, None, None, None, None],
   [None, None, None, None, None, None, None, None],
   [None, None, None, None, None, None, None, None],
   [None, None, None, None, ['white', '킹'], None, None, None],
]

for row in board:
   for piece in row:
       if piece:
           if piece[0] == 'black':
               print('▲', end=' ')
           else:
               print('△', end=' ')
       else:
           print('.', end=' ')
   print()
```

여러 컬렉션을 나란히 순회하고 싶다면, zip 함수 이용하기

코드 7-18

```python
seasons = ['봄', '여름', '가을', '겨울']
mountains = ['금강산', '봉래산', '풍악산', '개골산']

for season in seasons:
   for mountain in mountains:
       print(season + '에는 ' + mountain)


코드 7-20
seasons = ['봄', '여름', '가을', '겨울']
mountains = ['금강산', '봉래산', '풍악산', '개골산']

for season, mountain in zip(seasons, mountains):
   print(season + '에는 ' + mountain)

```

연습문제 7-4

```python
happiness = {
   '호주': 7.95,
   '노르웨이': 7.9,
   '미국': 7.85,
   '일본': 6.2,
   '한국': 5.75,
}

for country, happy in happiness.items():
   print(country + ' 사람들은 ' + str(happy) + '만큼 행복합니다.')
# 모범 답안에서는 print할 때 각 요소를 쉼표로 넣고, sep=''로 했음.
```

연습문제 7-5

```python
천간 = ['갑', '을', '병', '정', '무', '기', '경', '신', '임', '계']
지지 = ['자', '축', '인', '묘', '진', '사', '오', '미', '신', '유', '술', '해']

for a_천간 in 천간:
   for a_지지 in 지지:
       print(a_천간 + a_지지 + ' ', end='') # 모범 답안: print(간, 지, sep='', end=' ')
   print()
```

연습문제 7-6

```python
def plus_elements(seq1, seq2):
   """두 개의 시퀀스를 전달받은 후 각 요소를 순서대로 합한 리스트를 반환한다.
   ex) (1 , 2, 3) [4, 5, 6]   =>    [5, 7, 9] """

   result_list = []
   for number1, number2 in zip(seq1, seq2):
       result_list.append(number1 + number2)

   return result_list


print(plus_elements((1, 2, 3), [4, 5, 6]))
```

컬렉션에 많은 정보가 답겨 있어도 가공해야 새로운 지식도 이끌어 낼 수 있다.

새 리스트를 만들어 요소 수정해 넣기

코드 7-22

```python
prices = [2500, 3000, 1800, 3500, 2000, 3000, 2500, 2000]
new_price = []

for price in prices:
   new_price.append(price + 50)

print(new_price)
```

리스트 조건제시법이란, 각 요소를 구하는 조건을 제시하여 컬렉션을 정의하는 방법이다.

[2, 4, 6, 8, 10]의 리스트 표현 방법

- 원소나열법: [2, 4, 6, 8, 10]
- 레인지: 2 이상 11 미만의 2씩 증가하는 수의 리스트
- 조건제시법: [1, 2, 3, 4, 5]의 각 요소에 2를 곱한 리스트

조건제시법은 기존에 정의된 컬렉션이 있어야 한다.

코드 7-23

```python
[e * 2 for e in [1, 2, 3, 4, 5]]
[2, 4, 6, 8, 10]
```

코드 7-24

```python
prices = [2500, 3000, 1800, 3500, 2000, 3000, 2500, 2000]
print([price + 50 for price in prices])
```

리스트 조건제시법과 유사한 기능을 하는 함수 `map()`

리스트 조건제시법은 파이썬 문법에 정의된 ‘식'이고, map은 일반적인 ‘함수'이다.

코드 7-25

```python
prices = [2500, 3000, 1800, 3500, 2000, 3000, 2500, 2000]
def plus50(n):
   return n + 50

print(list(map(plus50, prices)))
```

코드 7- 26

```python
prices = [2500, 3000, 1800, 3500, 2000, 3000, 2500, 2000]
print(list(map(lambda a: a + 50, prices)))
```

코드 7-27

```python
prices = [2500, 3000, 1800, 3500, 2000, 3000, 2500, 2000]
total_price = 0
num_items = 0

for price in prices:
   total_price += price
   num_items += 1

print(total_price / num_items)
```

코드 7-28

```python
prices = [2500, 3000, 1800, 3500, 2000, 3000, 2500, 2000]
most_expensive = 0

for price in prices:
   if most_expensive < price:
       most_expensive = price

print(most_expensive)
```

컬렉션이 요소를 원하는 조건에 따라 선별하는 방법

코드 7-29

```python
prices = [2500, 3000, 1800, 3500, 2000, 3000, 2500, 2000]
filtered_prices = []

for price in prices:
   if price <= 2000:
       filtered_prices.append(price)

print(filtered_prices)
```

코드 7-30

```python
prices = [2500, 3000, 1800, 3500, 2000, 3000, 2500, 2000]
print([price for price in prices])
print([price for price in prices if price <= 2000])
```

코드 7-31

```python
prices = [2500, 3000, 1800, 3500, 2000, 3000, 2500, 2000]
print(list(filter(lambda price: price <= 2000, prices)))
```

기준을 정해 요소 정렬하기

거품정렬 진화과정…

코드 7-32

```python
coll = [2, 1]

if coll[0] > coll[1]:
   coll[0], coll[1] = coll[1], coll[0]
print(coll)
```

코드 7-33

```python
coll = [10, 5, 1]

if coll[0] > coll[1]:
   coll[0], coll[1] = coll[1], coll[0]
print(coll)

if coll[1] > coll[2]:
   coll[1], coll[2] = coll[2], coll[1]
print(coll)
```

코드 7-34

```python
coll = [10, 5, 1, 9, 7, 3]

for i in range(len(coll) - 1):
   if coll[i] > coll[ i + 1]:
       coll[i], coll[i + 1] = coll[i + 1], coll[i]

print(coll)



코드 7-35
coll = [10, 5, 1, 9, 7, 3]

for _ in coll:
   for i in range(len(coll) - 1):
       if coll[i] > coll[i + 1]:
           coll[i], coll[i + 1] = coll[i + 1], coll[i]

print(coll)
```

`sorted()` 함수 사용하기

코드 7-36

```python
print(sorted([10, 5, 1, 9, 7, 3]))
```

코드 7-37

```python
coll = [10, 5, 1, 9, 7, 3]
print(sorted(coll))
print(coll)

coll = sorted(coll)
print(coll)
```

코드 7-38

```python
print(sorted(['월', '화', '수', '목', '금']))
```

`sorted()` 함수가 반환하는 결과는 리스트이다.

코드 7-39

```python
print(sorted([5, 4, 3, 2, 1]))
print(sorted('안녕하세요'))
print(sorted({'사자', '박쥐', '늑대', '곰'}))
```

내림차순 정렬
코드 7-40

```python
print(sorted([3, 5, 1, 2, 4]))
print(sorted([3, 5, 1, 2, 4], reverse=True))
```

key 매개변수로 각 요소를 가공한 뒤 정렬하기

코드 7-41

```python
import pprint
print(sorted([3, -5, 1, -2, -4], key=abs))

items = [
   {'name': '아메리카노', 'price': 2000},
   {'name': '카페라테', 'price': 2500},
   {'name': '카푸치노', 'price': 2400},
]
sorted_items = sorted(items, key=lambda item: item['price'])
pprint.pprint(sorted_items)
```

연습문제 7-7(이 리스트를 1번, 레인지 방식, 2번. 조건제시법 방식 이렇게 두 가지 다 보이란 문제인 줄 착각함… 레인지만으론 도저히 만들 수 없을 거 같아서 연습문제 해답 봄..)

```python
number_list = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
comprehension_list = [n * n for n in number_list]

print(comprehension_list)
```

연습문제 7-7 모범답안 방식으로 수정

```python
comprehension_list = [n * n for n in range(10)]
```

연습문제 7-8

```python
def comprehension_list(n):
   return n * n

print(list(map(comprehension_list, range(10))))
```

연습문제 7-9

```python
def length(seq):
   """시퀀스(seq)를 하나 입력 받아 요소의 개수를 반환한다."""

   number = 0
   for _ in seq:
       number += 1

   return number

print(length([1, 2, 3, 4]))
```

연습문제 7-10

```python
def longest(*args):
   """여러 개의 시퀀스를 입력받아, 그중 가장 많은 요소를 가진 시퀀스를 반환한다."""

   longest_list = []

   for a_list in args:
       if len(longest_list) < len(a_list):
           longest_list = a_list
   return longest_list


print(longest([1, 2, 3], (4, 5), [], 'abcdefg', range(5)))
print(longest('파이썬', '프로그래밍'))
print(longest(range(10), range(100), range(50)))

# 모범 답안의 다른 예시
# def longest(*sequences):
#     return max(*sequences, key=len)
```

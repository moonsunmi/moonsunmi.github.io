---
layout: post
title:  "파이썬 웹 프로그래밍 실전편 공부 1(13장)"
date:   2022-08-29 22:45:13 +0900
categories: python-web-programming-practice
---

# 13장 Model

## 모델 정의 

models.py 파일에는 테이블을 정의하는 것이 기본.
ORM 방식에서는 관련 변수 메소드를 추가적으로 정의할 수 있는 장점이 있다.

테이블의 컬럼(장고에서는 테이블의 필드 또는 모델 필드라고 함)은 모델 클래스의 속성으로 정의, 테이블과 관련된 함수는 모델 클래스의 메소드로 정의

### 모델 속성

필드를 정의하려면 다음처럼.

descript = models.CharField(max_length=10)

* 필드명: `descript`
* 필드 타입: `models.CharField`
* 필드 옵션: `max_length=10`(`CharField` 타입에서 `max_length`는 필수)


#### 필드 타입의 역할
1. `descript` 컬럼의 타입을 지정한다. `CharField`는 `VARCHAR` 타입으로 반환된다.
2. 폼으로 렌더링되는 경우, HTML 위젯을 지정한다. `<input type="text">` 태그로
3. 필드 또는 폼에 대한 유효성 검사 시 최소 기준이 된다.


> 장고의 커스텀 필드 타입을 정의할 수도 있다. (장고 사이트 참고)


### 모델 메서드

장고에서는 객체 메서드만 사용한다. 즉, 모델 클래스에 정의하는 메서드는 모두 객체 메서드이며, 항상 `self` 인자를 가진다.

그러므로 레코드의 총 개수를 반환하는 것처럼, 테이블 레벨의 동작은 별도의 Manager 클래스를 정의해서 사용한다.

> `__str__()`는 항상 정의해 주는 게 좋다.


`get_absolute_url()`: 자신이 정의된 객체를 지칭하는 URL을 반환. URLconf에서 DetailView 제네릭 뷰를 사용하는 경우가 좋은 예.


### Meta 내부 클래스 속성

모델 클래스 필드는 아니지만, 모델 클래스에 필요하긴 한 항목을 Meta 내부 클래스에 정의한다.
(필드 속성과 그 외 속성을 따로 관리)

#### ordering
데이터베이스에서 가져온 데이터를 정렬해서 출력한다. 데이터베이스에 저장하는 순서가 아님.
`ordering = ['-pub_date', 'author']`
날짜 내림차순 정렬 후 저자 오름차순 정렬 순서

#### db_table
테이블 이름 지정. 지정하지 않으면 디폴트로 앱명_클래스명(소문자)로 지정된다.

#### verbose_name
모델 객체의 별칭.



### Manage 속성

모든 모델은 반드시 Manager 속성을 가진다. 디폴트 이름은 objects.

Manager 속성은 모델 클래스를 통해서만 액세스할 수 있다.(모델 객체를 통해서 불가)

QuerySet 객체를 반환하는 동작처럼 테이블 레벨에서의 기능은 Manager 클래스의 메서드로 이루어진다.

`Album.objects.all()`

* `Album`: 모델 클래스(모델 객체 아님)
* `objects`: Manager 속성명
* `all()`: Manager 클래스의 메서드

모델 클래스에서 Manager 속성은 여러 개 정의할 수 있다.


## 모델 간 관계

관계가 맺어진 양쪽 모두의 모델에서 정의가 필요한 것이 원칙이지만, 한쪽에서 관계를 정의했다면 상대편은 자동으로 정의를 해준다.


### 1:N 관계
N 모델에서 ForeignKey 필드를 정의하면서, 필수 인자로 1 모델을 지정한다.

```python
class Owner(models.Model):

class Album(models.Model):
    owner = models.ForeignKey('auth.User')
```

### N:N 관계
두 모델 중 어느 쪽에서 ManyToManyField를 정의해도 상관 없지만, 한쪽에만 정의해야 한다. 양쪽에 정의하면 안 된다.

```python
class Album(models.Model):
    # albums ManyToManyField 정의하지 않았음.

class Publication(models.Model):
    albums = models.ManyToManyField(Album)

```


### 1:1 관계
OneToOneField 필드 타입도 관계를 맺고자 하는 모델 클래스를 필수 인자로 지정한다. OneToOneField 필드 타입은 ForeignKey 필드에 unique=True 옵션을 준 것과 유사하다.

```python
class place(models.Model):
    
    
class restaurant(models.Model):
    place = models.OneToOneField(Place, on_delete=models.CASCADE)

```









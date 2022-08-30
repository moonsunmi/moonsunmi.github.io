---
layout: post
title:  "파이썬 웹 프로그래밍 공부 2(13장)"
date:   2022-08-30 22:31:24 +0900
categories: python-web-programming-practice
---

## 관계 매니저

모델 간 관계를 다루기 위한 클래스를 의미한다. 1:N, N:N 관계에서만 관계 매니저가 사용된다.

```python
class Owner(models.Model):

class Album(models.Model):
    owner = models.ForeignKey('auth.User')

user1.album_set  # 관계 매니저 클래스 객체
album1.owner     # 관계 매니저 클래스 객체
```

### 관계 매니저 메서드
실행 즉시 데이터베이스에 반영되어, save() 메서드를 호출할 필요가 없다.

#### add(*objs, bulk=True)
두 모델 객체 간에 관계를 맺어 주는 역할을 한다.

#### create(**kwargs)
새로운 객체를 생성해서 데이터베이스에 저장하고, 관계 매니저 클래스에 대한 객체의 집합에 넣는다.

#### remove(*objs, bulk=True)
두 모델 객체 간의 관계를 해제한다.(객체 삭제가 아님)

#### clear(bulk=True)
관계 매니저 클래스에 대한 객체의 집합에 있는 모든 객체를 삭제한다.

> remove, clear 모두 상대 객체를 삭제하는 게 아니라, 관계만 끊는다.

#### set(objs, bulk=True, clear=False)
add(), remove(), clear() 적절히 조합



## 단축 함수

웹 프로그램 시 자주 쓰이는 기능들은 장고에서 자체 함수로 제공한다.

### render()

템플릿 파일과 컨텍스트 사전을 인자로 받아 렌더링 처리 후, HttpResponse 객체를 반환하는 함수이다.

`render(request, template_name, context=None, content_type=None, status=None, using=None)`

* request(필수): 클라이언트로부터 보내온 요청 객체
* template_name(필수): 템플릿 파일명
* context: 템플릿 컨텍스트 데이터가 담긴 파이썬 사전형 객체

### redirect()
to 인자의 URL로 이동하기 위한 HttpResponseRedirect 객체를 반환한다.

redirect(to, *args, permanent=False, **kwargs)

* to: 이동하려는 URL

### get_object_or_404()
klass 모델에 해당하는 테이블에서 args 또는 kargs 조건에 맞는 레코드를 검색한다.

조건에 맞는 레코드가 둘 이상이거나 없으면 익셉션을 발생시킨다.

### get_list_or_404()
klass 모델에 해당하는 테이블에서 args 또는 kargs 조건에 맞는 레코드를 검색한다. 

해당 레코드들의 리스트를 반환한다.







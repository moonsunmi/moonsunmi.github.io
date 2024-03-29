---
layout: post
title: "<do-it 자바스크립트 입문> 3일 간의 공부"
date: 2022-09-18 22:34:31 +0900
tags: javascript
---

코딩 vs. 프로그래밍: 코딩은 소스코드를 작성하는 행위만을 말하고, 프로그래밍은 문제를 분석하고 해결방법을 찾는 과정까지 포함한다.

웹 프로그래밍: 웹 브라우저와 관련된 프로그램을 작성하는 것

`<script>` 태그는 보통 `</body>` 태그 바로 앞에 삽입한다.

'외부 스크립트 파일을 연결한다'는 말의 뜻:
자바스크립트 소스를 따로 작성하여 HTML 문서에 연결하는 것

외부 스크립트 파일을 연결하는 방법

```html
<script src="js/change.js"></script>
```

웹 브라우저에는 HTML 분석기, CSS 분석기, 자바스크립트 해석기가 포함되어 있다.
HTML 분석기: `<html>` 태그 사이의 내용을 해석. 주로 태그의 순서와 포함 관계를 확인한다. `<body>` 안에 세 개의 `<h1>` 태그가 있군... 등등.
CSS 분석기: `<style>` 태그 사이의 스타일 정보를 분석한다.
자바스크립트 해석기: `<script>` 태그 사이의 자바스크립트 소스를 해석한다.

자바스크립트는 파이썬과 달리 낙타 표기법을 쓴다.

#### == 연산자와 === 연산자

- 10 == "10" -> true
- 10 === "10" -> false

== 연산자는 자료형이 다를 때 자동으로 자료형을 변환하여 비교한다. 그러나 === 연산자는 변환을 허용하지 않는다.

#### 조건 연산자

```html
(score >= 60)? alert('합격'): alert('불합격');
```

#### falsy한 값: false로 인정할 수 있는 값

- 0
- ""
- NaN
- undefined
- null

- NaN: 값이 할당되지 않은 상태
- null: 유효하지 않은 값

if-else 조건문에서 if문은 조건을 무조건 확인하고, else는 if문이 false일 때만 실행된다. 그래서 true가 될 경우가 많은 조건을 if문에 넣는 게 실행시간이 더 짧아진다.

`var`와 `let`,`const`의 가장 큰 차이는 스코프 범위. `var`는 함수 영역의 스코프, `let`과 `const`는 블록 영역의 스코프.

JavaScript에서 호이스팅(hoisting)이란, 인터프리터가 변수와 함수의 메모리 공간을 선언 전에 미리 할당하는 것을 의미한다. `var` 로 선언한 변수의 경우 호이스팅 시 `undefined`로 변수를 초기화한다. `let`은 호이스팅이 없으므로 오류를 발생시킨다.

### 함수들

#### 익명함수

```html
var add = function(a, b){ return a + b; }
```

#### 즉시실행함수

```html
var result = (function(a , b) { return a + b; }(1, 2));
```

#### 화살표함수

```html
const hi = () => "안녕하세요?"; let hi3 = user => document.write(user + "님,
안녕하세요?"); let sum = (a, b) => {return a + b} let sum = (a, b) => a + b;
```

return 문은 생략 가능.

### 이벤트

이벤트와 이벤트 처리 함수를 연결해 주는 것을 이벤트 처리기 또는 이벤트 핸들러라고 한다. 이벤트 처리기는 이벤트 이름 앞에 on을 붙여 사용한다.

## 객체

자바스크립트에서 객체는 자료를 저장하고 처리하는 기본 단위. 여러 정보를 가지는 복합 자료형.

- 내장 객체: 자주 사용하는 요소는 이미 객체로 정의되어 있다. (Number, Boolean, Array, Math 등)
- 문서 객체 모델: 객체를 사용해 웹 문서를 관리하는 방식. 웹 문서 자체를 담는 Document 객체, 웹 문서 안의 이미지를 관리하는 Image 객체 등
- 브라우저 객체 모델: 웹 브라우저의 주소 표시줄이나 창 크기 등 웹 브라우저 정보를 객체로 다루는 것.
  - Navigator 객체: 사용 중인 브라우저 종류나 버전을 담고 있다.
  - History 객체: 브라우저에서 방문한 기록을 남긴다.
  - Location 객체: 주소 표시줄 정보를 담는다.
  - Screen 객체: 화면 크기 정보가 들어 있다.
- 사용자 정의 객체

Window 객체는 모든 객체를 품고 있는 최상위 객체이기 때문에 함수 이름만 사용해서 실행해도 된다.

```html
window.alert('same'); alert('same');
```

자바스크립트에는 클래스라는 개념이 없다. 그래서 기존의 객체를 복사해(cloning) 새로운 객체를 생성하는 프로토타입 기반의 언어이다. 프로토타입 기반 언어는 객체 원형인 프로토타입을 이용해 새로운 객체를 만들어 낸다.

```
var now = new Date()
```

new 예약어 뒤에 Date라는 프로토타입 객체 이름과 괄호를 써 준다. now는 이제 Date 객체의 인스턴스이므로, Date 객체에서 정의한 속성과 함수를 모두 사용할 수 있다.

리터럴을 사용해서 표기한다는 것은 변수를 선언하면서 동시에 값을 지정해 주는 표기 방식을 말한다.
`var a = 10;`

### 리터럴 표기법으로 객체 만들기

객체 리터럴은 객체를 선언하면서 동시에 값을 지정해 주는 것

```html
var book = { title: "자바스크립트", price: 15000, info: function(){
alert(this.title + '책' + this.price + '원'); } };
```

### 생성자 함수를 사용해 객체 만들기

```html
function Book(author, pages, title){ this.author = author; this.pages = pages;
this.title = title; } jsBook = new Book("go", 200, "javascript");
```

function Book() { ... }를 Book 객체 생성자라고 부른다.

getMonth() 함수의 반환 값은 0부터 11 사이의 숫자이기 때문에 +1을 해줘야 한다.

자바스크립트 객체 안에 미리 정의되어 있는 정보를 속성이라고 부르며, 미리 정의되어 있는 함수를 메서드라고 부른다.

## Array 객체

- 둘 이상의 배열을 연결: concat()
- 배열 요소를 구분기호로 연결: join()
- 새로운 요소를 추가
  - 맨 끝에 추가: push()
  - 맨 앞에 추가: unshift()
- 기존 요소를 추출
  - 맨 뒤 요소 추출: pop()
  - 맨 앞 요소 추출: shift()
- 원하는 요소를 삭제하거나 추가(배열 실제로 수정): splice()
  - splice(2): 인덱스 2부터 끝까지 삭제
  - splice(2, 3): 인덱스 2부터 3개를 삭제
  - splice(3, 1, "js"): 인덱스 3부터 1개를 삭제 후, "js"를 추가
  - splice(2, 0, "js"): 인덱스 2 위치에 "js"를 추가(삭제 x)
- 시작 인덱스에서 끝 인덱스 사이의 요소를 추출(배열 변화 없음): slice()
  - slice(2, 3): 인덱스 2부터 인덱스 3까지 추출

## 문서 객체 모델(DOM)

DOM: 웹 문서의 모든 요소를 자바스크립트를 이용해 조작할 수 있도록 객체를 사용해 문서를 해석하는 방법.

- 웹 문서의 태그는 요소(Element) 노드로 표현한다.
- 태그가 품고 있는 텍스트는 텍스트(Text) 노드로 표현한다.
- 태그의 속성은 모두 속성(Attribute) 노드로 표현한다.
- 주석은 주석(Comment) 노드로 표현한다.

```html
<p class="accent">주문이 완료되었습니다.</p>
```

- 요소 노드: p
- 속성 노드: class="accent"
- 텍스트 노드: 주문이 완료되었습니다.

- getElementById(): DOM 요소를 id 선택자로 접근하는 함수
- getElementsByClassName(): Dom 요소를 class 값으로 찾아내는 함수
- getElementsByTagName(): DOM 요소를 태그 이름으로 찾아내는 함수
- querySelector(), querySelectorAll(): ID, 클래스 이름, 태그 이름 다 사용할 수 있다.
  - ID: '#id'
  - 클래스: '.class'
- getAttribute(), setAttribute(): 태그 속성을 가져오거나 수정하는 함수

> this
> 자바스크립트 내에서 this는 '누가 나를 불렀느냐'를 뜻한다고 합니다. 즉, 선언이 아닌 호출에 따라 달라진다는 거죠.

```html
var bigPic = document.getElementById("cup"); var smallPics =
document.getElementsByClassName("small"); for (let i=0; i < smallPics.length;
i++){ smallPics[i].onclick = showBig; // --> <img onclick="showBig" />이 되는
것임. } function showBig(){ // smallPics를 통해 이 함수가 호출되었으므로 this는
smallPics[i]의 객체가 된다. bigPic.setAttribute("src", this.src); }
```

### DOM에서 이벤트 처리하기

addEventListener(): 이벤트가 발생한 요소에 이벤트 처리기를 연결해 주는 함수.

```html
var pic = document.querySelector('#pic'); pic.addEventListener("mouseover",
changePic, false);
```

pic 변수에 지정된 요소에 이벤트 유형을 지정한다. (이벤트유형, 실행할함수, 캡처여부)

DOM으로 CSS 속성에 접근할 때, background-color처럼 하이픈이 포함된 속성은 backgroundColor로 표기한다.

### DOM 트리에 새로운 노드 추가, 삭제하기

- 노드를 추가하려면 해당 노드를 만든 후 부모 노드에 연결해 줘야 한다.
- 노드는 스스로 자신을 삭제할 수 없기 때문에 부모 노드에 접근한 후 부모 노드에서 삭제해야 한다.

### 이름(name 값)으로 접근하기

```html
<form name="ship">
  <input type="checkbox" id="shippingInfo" name="shippingInfo" />
</form>
```

document.ship.shippingInfo
document.forms["ship"].elements["shippingInfo"]

- id 식별자를 통해 접근하는 방법은 접근할 요소에 id 속성이 지정되어 있기만 하면 되지만, name 속성을 사용해 접근하려면 <form> 태그뿐 아니라 접근하려는 폼 요소에 모두 name 속성이 지정되어 있어야 한다.

## 브라우저 객체 모델

브라우저 객체 모델은 자바스크립트 프로그램을 통해 브라우저 창을 관리할 수 있도록 브라우저 요소를 객체화해 놓은 것을 가리킨다.

- Window 객체: 브라우저 창이 열릴 때마다 하나씩 만들어진다. 브라우저 창 안에 존재하는 모든 요소의 최상위 객체
- Document 객체: <body> 태그를 만나면 만들어지는 객체. HTML 문서 정보를 가지고 있다.
- History 객체: 현재 창에서 사용자의 방문 기록을 저장한다.
- Location 객체: 현재 페이지에 대한 URL 정보를 가진다.
- Navigator 객체: 현재 사용 중인 웹 브라우저 정보를 가리고 있다.
- Screen 객체: 현재 사용 중인 화면 정보를 다룬다.

렌더링 엔진: 브라우저에서 웹 문서를 화면에 표시하기 위해 웹 문서의 태그와 스타일을 해석하는 프로그램. 웹 브라우저마다 내장된 렌더링 엔진이 다르기 때문에 HTML이나 CSS를 해석하는 방법이 다르다.

자바스크립트 엔진: 자바스크립트 소스를 해석

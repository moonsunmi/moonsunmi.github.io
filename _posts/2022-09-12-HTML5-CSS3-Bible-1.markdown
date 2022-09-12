---
layout: post
title:  "HTML5 CSS3 바이블 공부 1"
date:   2022-09-10 22:50:13 +0900
categories: python_web_programming_practice
---

태그는 HTML 페이지에서 객체를 만들 때 사용하는 것. 태그를 사용해 만들어진 객체를 요소라고 한다.

head 태그는 body 태그에서 필요한 스타일시트와 자바스크립트를 제공하는 데 사용한다.

h1~ h6까지 heading을 의미하며, 크기와 우선순위를 나타낸다.

## 2.3 글자 태그

p: 단락 태그


### 앵커 태그
서로 다른 웹 페이지 사이를 이동하거나 웹 페이지 내부에서 특정한 위치로 이동할 때 사용하는 태그 (a 태그)

이동시키지 않는 태그를 만들려면 `<a href="#"></a>`로 해야 한다.

페이지 내부의 원하는 장소로 이동하려면 이동하려는 곳에 id 속성을 부여 후 `<a href="#id'>`로 해 준다. 



## 2.4 목록 태그

내비게이션 메뉴: 페이지를 이동할 때 사용되는 메뉴

img 태그에서 존재하지 않는 이미지가 나올 때 alt 속성값을 주면, 메시지가 출력된다.

음악 첨부하기
`<audio src="Kalimba.mp3" controls="controls" autoplay="autoplay"></audio>`

음악 파일 형식마다 미세하게 다른 문제가 생길 수 있기에 다음과 같이 대처한다.

`
    <audio controls="controls">
        <source src="Kalimba.mp3" type="audio/mp3" />
        <source src="Kalimba.ogg" type="audio/ogg" />
    </audio>
`

type 속성을 입력하지 않으면 트래픽 낭비가 되므로 결과에 변화가 없더라도 꼭 지정해 주자.


#### 비디오 태그는 video.
동영상이 대기 상태일 때 표시할 이미지는 poster 속성으로 지정해 줄 수 있다.

video 태그는 웹 브라우저마다 일관되지 않게 표시되고, 익스플로러 8 이하에서 동작하지도 않는다. 이러한 문제를 해결할 수 있는 게 video.js 플러그인. -> 해당 사이트에 들어가서 사용법대로 해 보면 된다.



#### form 태그의 속성

action: 입력 데이터의 전달 위치를 지정한다.
method: 입력 데이터의 전달 방식을 선택한다.

label 태그는 input 태그를 설명하는 데 사용됩니다.

`
<label for="name">이름</label>
<input name="name" type="text" />
`

#### select 태그

선택사항을 그룹화하고 싶으면 optgroup으로 지정한다.

요즘에는 안 예뻐서 잘 사용하지 않는다. 자바스크립트를 사용해 하나하나 만든다.


### 2장 연습문제

{% raw %}
```html
<!DOCTYPE html>
<head>
    <title>hobby</title>
</head>

<body>
    <label for="name">이름:</label>
    <input id="name" type="text" />

    <br />
    성별:
    <input type="radio" />남성
    <input type="radio" />여성
    <br />

    취미:
    <input type="checkbox" />운동
    <input type="checkbox" />음악
    <input type="checkbox" />개발
    <br />

    <input type="submit" />
</body>
</html>
```
{% endraw %}


## 3장

CSS3 선택자는 특정한 HTML 태그를 선택할 때 사용하는 기능이다.

style 태그 내부에 입력되는 코드를 스타일시트라고 부른다.



### 아이디 선택자

`#아이디`: 해당 아이디 속성을 가지고 있는 태그를 선택한다. id 속성은 웹 페이지 내부에서 중복되면 안 되므로, 특정한 하나의 태그를 선택할 때 사용한다.

`.클래스`: 해당 클래스를 가지고 있는 태그를 선택한다.

`태그.클래스`로 지정하면 해당 태그의 클래스일 때만 스타일이 적용된다.


속성 선택자: `태그[속성=속성값]`

`input[type=text] { background-color: aquamarine;}`


후손 선택자: 
`#section li {color: yellow;}`

* table은 tbody가 자동으로 생성되므로 헷갈려서 자손 선택자 사용하지 않는 게 좋다. 


동위 선택자: 동위 관계에서 뒤에 위치한 태그를 선택할 때 사용한다.
h1과 h2는 동위 관계인데, h1 + h2 { color: red; } 이렇게 되어 있으면, h1 태그 바로 뒤에 있는 h2에 속성을 적용한다. h1 ~h2는 h1 태그 뒤에 있는 h2 태그들에 적용한다.


반응 선택자: 사용자의 반응으로 특정한 상태가 되는 선택자입니다.
* `h1:hover { color: red; }` : 마우스 올릴 때
* `h1:active { color: blue; }` : 마우스 클릭할 때


상태 선택자: 입력 양식의 상태를 선택할 때 사용한다.
```
input:enabled { background-color: white; }
input:disabled { background-color: gray; }
input:focus { background-color: orange; }
```


구조 선택자: 특정한 위치에 있는 태그를 선택하는 선택자




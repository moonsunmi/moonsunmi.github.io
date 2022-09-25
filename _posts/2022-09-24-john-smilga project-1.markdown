---
layout: post
title:  "John-smilga Project"
date:   2022-09-24 22:18:07 +0900
categories: HTML+CSS
---

|           | 뽀모도리  |
|-----------|-------|
| 일간(달성/목표) | 11/16 |
| 주간(달성/목표) | 35/40 |



### visual studio code에서 유용한 단축키
> * shitf + option + 아래화살표: 한줄 복사붙여넣기
> * option + 화살표 : 한줄 이동




1. 폰트를 고른다.
2. 디폴트 스타일을 세팅

다음 사이트 유용하다.
* fontpair.co : 폰트와 디자인의 조합을 보여줌(무료인 것들?)
* type-scale.com : 태그 단계별 적절한 폰트 사이즈를 지정해둔 css를 제공한다. - 원하는 폰트를 고르면 css를 자동으로 작성해 주는데, 그걸 이용한다.

### type-scale.com의 css


복사한 css는 다음과 같이 되어 있는데, 
```css
h1, h2, h3, h4, h5 {
  margin: 3rem 0 1.38rem;
  font-family: 'Spectral', serif;
  font-weight: 400;
  line-height: 1.3;
}
```

hard coding 부분을 수정한다. 다음의 root 추가

```css
:root{
    --headingFont: 'Spectral', serif;
    --bodyFont: 'Spectral', serif;
    --smallText: 0.7em;
}
```
> :root는 문서 트리의 루트 요소를 선택하며, HTML의 루트 요소는 <html> 요소이므로, :root와 html은 같다.


상단의 css는 다음과 같이 수정

```css
h1, h2, h3, h4, h5 {
  margin: 3rem 0 1.38rem;
  font-family: var(--headingFont);
  font-weight: 400;
  line-height: 1.3;
}
```


margin에서 합쳐 있는 값은, 분리해서 놓는 걸 추천
```css
  margin: 3rem 0 1.38rem;
```

```css
  margin: 0;
  margin-bottom: 1.38rem;
```


### primary color 선택하기

primary color 값은 다음 사이트들 이용하면 좋다.
색을 관리하기 위해서는 hex 코드보다는 hsl을 이용하는 게 더 좋다. shl(244, 100%, 94%)처럼 퍼센트로 관리할 수 있어서.


#### Manual Approach
* coolors
* happyhues
* select your own color
* shade는 여기에서: shadowlord

#### Library/Faster Approach
* bootstrap
* tailwind


### grey 선택하기
* tailwind


body의 background 값을 넣을 때, 다음과 같은 형태로 할 수도 있긴 하지만,

```css
body {
  background: var(--grey-100);
}
```

관리를 편하게 하기 위해서 다음을 추천한다.

```css
:root{
    ...
    /* rest of the colors */
    --backgroudColor:var(--grey-50);
    --textColor: var(--grey-900)
}

body {
  background: var(--backgroudColor);
  color: var(--textColor);
  ...
}

```
* `--samllText: 0.7em;`: 작은 텍스트 사이즈
* `--borderRadius: 0.25rem;`: 박스의 꼭지점 라운딩값


> * `*,::after,::before{box-sizing: border-box;}`: 모든 태그들의 앞뒤로 다음을 값을 설정한다.
> * `box-sizing: border-box`: 박스의 width 값 등을 주면, 텍스트 영역이 width가 되고, 테두리가 더해져서 실제 차지하는 width는 더 커진다. 이렇게 지정해 두면, 테두리값까지 포함한 width를 지정할 수 있기 때문에 모든 엘리멘트에 이 값을 지정해 주고 있다.


* content 추가할 때마다, max-width를 지정해 주고 있다.

* `--transition: 0.3s ease-in-out all;` 마우스를 사진 위에 올리고 그러면, 조금 어두워지거나 하는 효과들.

* 박스에 커서를 올리면 shadow가 조금 더 짙어지게 한다. tailwindcss에서 Box Shadow 이펙트 확인

* vw: width의 1% 값. width가 1000px이라면 1vw는 10px. width가 100px이라면, 1vw는 1px


```css
.img{
    width: 100%;
    display: block;
    object-fit: cover;
}
```
img에서 `display: block` 속성을 지정해 주지 않으면, 그림을 container로 감쌌을 경우 테두리와 그림 사이에 이상한 공백이 생기는데, 이걸 없애 준다.


부모의 폰트를 그대로 물려 받는다. body 태그에 지정된 폰트를 쓰는 것.
```css
::placeholder{
    font-family: inherit;
}
```


# FASTEST Way to Learn Web Development

https://www.youtube.com/watch?v=C-EHoNfkoDM

## 프론트엔드 배우기

w3schools.com
HTML은 HTML graphics까지 배운다.
CSS는 CSS Grid까지
JS HTML Dom까지


### CSS frameworks
* bootstrap: 웹사이트가 많이 비슷해 보임. 
* Tailwind CSS: 정식 웹사이트 document에서 'core concept'를 공부하면 됨.(정식 유튜브도 있음)


### Javascript frameworks
1. REACT 정식 웹사이트에서 제공하는 튜토리얼이 있다. 이걸 다 따라한다.
https://ko.reactjs.org/tutorial/tutorial.html

2. 튜토리얼로 tic-tac-toe 만들고 난 후에 Traversy Media 채널에 있는 Brad의 비디오를 보라.

3. SonnySanoha 채널의 Linkedin clone 코딩
Redux와 Firebase를 다룰 수 있다.

4. Sahil Gaba 비디오. NextJS를 이용해서 Hulu and Facebook clone 코딩한다.

5. 웹 사이트 프로젝트를 만들어서 포트폴리오를 만든다.
    * sony 채널에서 여러 클론 프로그래밍을 하든가, 내 생활을 편하게 해줄 만한 걸 만들던가.

## 백엔드 배우기
더 배우고 싶다면 백엔드 생각하라고. 백엔드 쪽으로 생각하고 싶을 때 다시 보자.




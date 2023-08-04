---
layout: post
title: "<do-it 자바스크립트 입문> 실전 프로젝트 1~2"
date: 2022-09-19 22:34:31 +0900
categories: javascript
---

### [실전 1] 숫자 맞히기 게임

```js
let answerNumber = Math.floor(Math.random() * 100) + 1;
let totalNumber = 0;

document.querySelector("#try").onkeypress = function (e) {
  if (e.keycode == 13 || e.which == 3) {
    // 파이어폭스는 which로 확인함.
    finding();
    return false;
  }
};

function finding() {
  console.log(answerNumber);
  console.log(totalNumber);

  let tryNumber = document.querySelector("#try").value;

  if (tryNumber >= 1 && tryNumber <= 100) {
    if (tryNumber != answerNumber) {
      console.log(tryNumber);
      if (tryNumber < answerNumber) {
        document.querySelector("#display").innerText = "UP";
      } else {
        document.querySelector("#display").innerText = "DOWN";
      }

      tryNumber = document.querySelector("#try").value = "";
      document.querySelector("#try").focus();
    } else {
      document.querySelector("#display").innerText = "맞혔습니다.";
    }
    totalNumber++;
    document.querySelector("#counter").innerText = "시도횟수: " + totalNumber;
  } else {
    alert("1~100 사이의 숫자를 넣으세요.");
  }
}
```

- Math.random() 함수는 '0 이상 1 미만'의 값을 전달한다. 따라서 1과 100 사이의 숫자를 체크하려면 +1을 해 줘야 한다. 경계값의 포함 여부를 꼭 확인해 보자.

### [실전 2] 이미지 슬라이드 쇼

```js
let pics = document.querySelectorAll("#container > img");
let showPicIndex = 0;
showSlide();
document.querySelector("#prev").onclick = prevSlide;
document.querySelector("#next").onclick = nextSlide;

function showSlide() {
  for (let i = 0; i < pics.length; i++) {
    pics[i].style = "display: none";
    //console.log(pics[i].style)
  }
  pics[showPicIndex].style = "display: block";
}

function prevSlide() {
  if (showPicIndex == 0) {
    // 맨 앞에서 앞으로 버튼을 누르면, 맨 뒤로
    showPicIndex = pics.length - 1;
  } else {
    showPicIndex--;
  }
  showSlide();
}

function nextSlide() {
  if (showPicIndex == pics.length - 1) {
    // 맨 뒤에서 뒤로 버튼을 누르면, 맨 앞으로
    showPicIndex = 0;
  } else {
    showPicIndex++;
  }
  showSlide();
}
```

- `let pics = document.querySelectorAll("#container > img");`에서 >는 '부모 선택자 > 자식 선택자'

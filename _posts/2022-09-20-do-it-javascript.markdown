---
layout: post
title: "<do-it 자바스크립트 입문> 실전 프로젝트 3~4 "
date: 2022-09-20 22:45:12 +0900
tags: javascript
---

### [실전 3] 온도 변환기

```js
// Celsius 섭씨(C) &#8451;   Fahrenheit 화씨(F) &#8457;
// (0°C × 9/5) + 32 = 32°F => (섭씨 * 9/5) + 32 = 화씨

//  unit = {celsius: '&#8451;', fahrenheit: '&#8457;'};
let unit = { celsius: "℃", fahrenheit: "℉" };

document.querySelector("#s-value").onkeyup = showTarget;
document.querySelector("#exchange").onclick = exchangUnit;

function exchangUnit() {
  // 단위를 바꾼다.
  let sUnit = document.querySelector("#s-unit");
  let tUnit = document.querySelector("#t-unit");

  let sUnitValue = sUnit.innerText;
  let tUnitValue = tUnit.innerText;

  sUnit.innerText = tUnitValue;
  tUnit.innerText = sUnitValue;

  document.querySelector("#s-value").value = "";
  document.querySelector("#t-value").value = "";
}

function showTarget() {
  // source 값을 기준으로 target 값을 계산해 표시한다.
  let sValue = this.value;

  if (document.querySelector("#s-unit").innerText == unit["celsius"]) {
    document.querySelector("#t-value").value = getCvalue(sValue);
  } else {
    document.querySelector("#t-value").value = getFvalue(sValue);
  }
}

function getCvalue(Fvalue) {
  // 화씨(F)를 섭씨(C)로 변환한 값을 리턴한다.
  // 섭씨 = (화씨 - 32) / (9/5)
  let Cvalue = (Fvalue - 32) / (9 / 5);
  return Cvalue.toFixed(2);
}

function getFvalue(Cvalue) {
  // 섭씨(C)를 화씨(F)로 변환한 값을 리턴한다.
  // 화씨 = (섭씨 * 9/5) + 32
  let Fvalue = (Cvalue * 9) / 5 + 32;
  return Fvalue.toFixed(2);
}
```

저자의 코드보다 10줄 이상 길다. 나는 섭씨와 화씨 가격을 반환하는 함수를 따로 만들었다.

### [실전 4] 라이트 박스

```js
let pics = document.querySelectorAll(".pic");

for (let i = 0; i < pics.length; i++) {
  pics[i].addEventListener("click", showPic);
}

function showPic() {
  let location = this.getAttribute("data-src");

  let lightboxImage = document.querySelector("#lightboxImage");
  lightboxImage.setAttribute("src", location);
  lightbox.style.display = "block";
  lightbox.onclick = function () {
    lightbox.style = "display:none;";
  };
}
```

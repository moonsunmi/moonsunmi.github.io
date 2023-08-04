---
layout: post
title: "<do-it 자바스크립트 입문> 실전 프로젝트 5~6"
date: 2022-09-21 22:49:18 +0900
categories: javascript
---

### [실전 5] 로컬 스토리지

#### 로컬 스토리지 알아보기

웹 스토리지의 자료들도 웹 사이트를 서핑하거나 브라우저 탭 을 닫으면 웹 사이트와 관련된 정보들을 저장한다. 하지만 사용자가 웹 스토리지의 정보를 서버로 전송하지 않는 이상 서버에서 사용자 PC로 들어와 스토리지 정보를 읽어가지 못하고, 쿠키보다 용량도 크다.

- 세션 스토리지: 브라우저 창이나 탭을 닫을 때까지를 하나의 세션(session)이라고 하는데, 세션 동안의 자료를 기억하고 있다가 세션이 끝나면(즉 브라우저 창이 닫히거나 탭이 닫히면) 자료를 모두 지워버린다.

- 로컬 스토리지: 세션이 끝나도 계속해서 자료를 저장할 수 있는 스토리지.

#### 로컬 스토리지 다루는 법

배열을 로컬 스토리지에 저장할 때는 `JSON.stringify()` 메서드를 사용하여 배열에 있는 여러 값을 하나의 문자열처럼 변환해서 저장한다. 그리고 로컬 스토리지에 있는 여러 값으로 된 문자열을 가져와 배열 형태로 변환할 때는 `JSON.parse()` 메서드를 사용한다.

배열 저장하기

```js
let colors = ["black", "blue", "red"];
localStorage.setItem("myColor", JSON.stringify(colors));
```

저장된 배열 가져오기

```js
let newColors = JSON.parse(localStorage.getItem("colors"));
```

이미 쇼핑몰 목록이 구현되어 있었고, 나는 로컬스토리지 기능만 추가했다. 로컬스토리지 기능을 위해서는 위에 적은 JSON.stringify와 parse 메서드를 사용해야 한다. stringify라는 단어가 있나? 싶어서 찾아 보니... JSON에서 만든 꽤 그럴 듯한 메서드 이름일 뿐인듯...

```js
var itemList = [];
itemList = getLocalStorage();
showList();

console.log(itemList);

var addBtn = document.querySelector("#add");
addBtn.addEventListener("click", addList); // addBtn.onclick = addList; 라고 해도 됨

function addList() {
  console.log("add");
  var item = document.querySelector("#item").value; // 텍스트 필드 내용 가져옴
  if (item != null) {
    itemList.push(item); // itemList 배열의 끝에 item 변수 값 추가
    saveLocalStorage(itemList); // localStorage에도 배열 형태로 저장한다. localStorage.setItem("itemList", itemList);
    document.querySelector("#item").value = ""; // id=”item”인 요소의 값을 지움
    document.querySelector("#item").focus(); // 텍스트 필드에 focus( ) 메서드 적용
  }
  showList();
}

function getLocalStorage() {
  let savedList = JSON.parse(localStorage.getItem("itemList"));
  if (savedList == null) return [];
  else return savedList;
}

function saveLocalStorage(itemList) {
  localStorage.setItem("itemList", JSON.stringify(itemList));
}

function showList() {
  console.log("showLIst");
  var list = "<ul>"; // 목록을 시작하는 <ul> 태그 저장
  for (var i = 0; i < itemList.length; i++) {
    // 배열 요소마다 반복
    list +=
      "<li>" + itemList[i] + "<span class='close' id=" + i + ">X</span></li>"; // 요소와 삭제 버튼을 <li>~</li>로 묶음
  }
  list += "</ul>"; // 목록을 끝내는 </ul> 태그 저장

  document.querySelector("#itemList").innerHTML = list; // list 내용 표시

  var remove = document.querySelectorAll(".close"); // 삭제 버튼을 변수로 저장. 배열 형태가 됨
  for (var i = 0; i < remove.length; i++) {
    // remove 배열의 요소 모두를 확인
    remove[i].addEventListener("click", removeList); // 요소를 클릭하면 removeList() 실행
  }
}

function removeList() {
  var id = this.getAttribute("id"); // this(클릭한 삭제 버튼)의 id 값 가져와 id 변수에 저장
  itemList.splice(id, 1); // itemList 배열에서 인덱스 값이 id인 요소 1개 삭제
  saveLocalStorage(itemList);
  showList(); // 변경된 itemList 배열을 다시 화면에 표시
}
```

### [실전 6] 타이머

- 1초: 1000밀리초
- `setInterval(반복함수, 간격)` 메서드는 간격(밀리초 단위)만큼 시간을 두고 '반복함수'를 계속 실행한다.
- `clearInterval(setInterval())` 메서드는 주어진 setInterval을 멈춘다.

```js
let leftTime = 0;
let timer;

document.querySelector(".start-btn").onclick = startTimer;
document.querySelector(".reset-btn").onclick = stopTimer;

function startTimer() {
  // 남은 시간을 표시한다.
  leftTime =
    Number(document.querySelector("#startMin").value * 60) +
    Number(document.querySelector("#startSec").value);
  timer = setInterval(ticking, 1000);
}

function stopTimer() {
  // 반복을 멈추고, 타이머 마무리 작업을 한다.
  document.querySelector("#startMin").value = "";
  document.querySelector("#startSec").value = "";
  document.querySelector("#display").innerText = "타이머 종료";
  clearInterval(timer);
}

function ticking() {
  // 1초가 지나갈 때마다 호출되는 함수
  if (leftTime <= 0) {
    stopTimer();
  } else {
    leftTime -= 1;
    document.querySelector("#display").innerText =
      parseInt(leftTime / 60) + "분 " + (leftTime % 60) + "초 남았습니다."; //leftTime + '남았습니다.';
  }
}
```

모범 답안에서는 초 단위에서의 카운팅과 분 단위에서의 카운팅을 따로 했다. 나는 그냥 leftTime값을 이용해 분과 초를 계산해서 출력했다. 프로그래밍하던 중 값을 확인하기 위해 `console.log`를 몇 개 추가했더니, 1초보다 많은 시간과 적은 시간이 왔다갔다 하면서 카운팅되었다. 이 프로그램이 정확하게 1초마다 카운팅되는 걸 보장해서 실전에서도 쓸 만한 상태인지는 잘 모르겠다.

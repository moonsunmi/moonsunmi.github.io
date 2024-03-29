---
layout: post
title: "점프 투 장고 공부 4"
date: 2022-08-20 22:50:10 +0900
tags: jumptodjango
---

## 초보자를 위한 HTML5

> - 태그 유형: 블록 레벨 요소, 인라인 레벨 요소
> - 블록 레벨 요소: 콘텐츠의 양과 무관하게 하나의 요소가 혼자서 한 줄을 통째로 차지
> - 인라인 레벨 요소: 태그에 포함된 콘텐츠의 크기만큼만 공간을 차지
> - 전역 속성(Global attributes)이란 모든 HTML 태그에서 공통적으로 사용할 수 있는 속성. `id`, `class`, `title`은 사용 빈도가 높은 전역 속성.
> - 고유한 식별자, `id`: 하나의 요소는 하나의 id를 가질 수 있으며, 각 요소에 대한 id는 고유한 값이어야 합니다.
> - 그룹화 식별자, `class`:class 속성은 여러 요소에 같은 이름의 식별자를 지정하여 요소를 그룹화할 수 있는 식별자
> - 요소가 두 개 이상의 class 값을 가지길 원한다면, `<p class="text content">슬프고 힘들어요</p>`
> - 요소를 설명하는 추가 텍스트, `title`
> - 요소와 관련된 추가 정보를 제공. 렌더링 되고 난 후의 결과물에 마우스를 얹었을 때 툴팁(tooltip)이 나타나게 됩니다.

열려 있는 웹 브라우저에서 개발자 도구를 열기: 맥 운영체제에서는 option+comman+I

## HTTP 서버 요청 - 응답 과정

1. 브라우저가 URL에 요청
2. 미들웨어: 서버가 요청을 처리하기 전에 미리 수행해야 할 작업들을 실행
3. 라우팅: 서버가 URL을 해석하고, 요청을 처리할 수 있는 핸들러(뷰)를 골라서 실행
4. 뷰: 요청을 처리

- 데이터 모델에서 필요한 정보를 가져오거나 기록
- 데이터 컨텍스트를 만들어 템플릿에 전달

5. 템플릿: 응답할 HTML 결과물을 렌더

- 컨텍스트를 이용해서 결과물 생성

6. 브라우저가 HTML을 화면에 그림

### 파이참 단축키

- option + 클릭: 자동 임포트
- command + 클릭: 함수가 정의된 곳으로 이동

```html
{% raw %} {% csrf_token %}
<!-- 오류 표시 start -->
{% if form.errors %}
<div class="alert alert-danger" roll="alert">
  {% for field in form %} {% if field.errors %}
  <div>
    <strong>{{ field.label }}</strong>
    {{ field.errors }}
  </div>
  {% endif %} {% endfor %}
</div>
{% endif %}
<!-- 오류 표시 end -->
{% endraw %}
```

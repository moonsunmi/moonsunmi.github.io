---
layout: post
title:  "점프 투 장고 공부 5"
date:   2022-08-21 22:39:25 +0900
categories: jumptodjango
---

## 3장 파이보 서비스 개발!

### 3-01 내비게이션바

navbar.html
```
<!-- 내비게이션바 start -->
<nav class="navbar navbar-expand-lg navbar-light bg-light">
   <div class="container-fluid">
       <a class="navbar-brand" href="{% url 'pybo:index' %}">Pybo</a>
       <button class="navbar-toggler" type="button"
               data-bs-toggle="collapse"
               data-bs-target="#navbarSupportedContent"
               aria-controls="navbarSupportedContent"
               aria-expanded="false"
               aria-label="Toggle navigation">
           <span class="navbar-toggler-icon"></span>
       </button>
       <div class="collapse navbar-collapse" id="navbarSupportedContent">
           <ul class="navbar-nav me-auto mb-2 mb-lg-0">
               <li class="nav-item">
                   <a class="nav-link" href="#">로그인</a>
               </li>
           </ul>
       </div>
   </div>
</nav>

<!-- 내비게이션바 end -->
```


navbar.html 파일은 다른 템플릿들에서 중복되어 사용되지는 않지만 독립된 하나의 템플릿으로 관리하는 것이 유지 보수에 유리하므로 분리하였다.



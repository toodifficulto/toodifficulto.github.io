---
layout: post
title: HTML 기본 구조
tags:  [python-web-development]
---
# HTML 기본 구조

가장 상단의 **header** 는 페이지마다 반복되는 머리말 부분이다. 주로 제목이나 로고, 짧은 소개말 등이 들어가고 **header** 라는 태그를 사용할 수 있다.

**navigation bar** 는 다른 페이지로 이동할 수 있는 메뉴 버튼이나 링크 등이 들어간다. header에 네비게이션 바가 포함되기도 한다. 이 부분을 마크업하기 위해서는 **nav** 라는 태그를 사용할 수 있다.

**main** 콘텐츠는 본문의 주요 콘텐츠 블럭이다. **main** 태그를 사용할 수 있다. **main 태그는 문서에서 한번만 사용해야 한다.**

**side bar** 영역은 기타 정보나 링크, 광고 등의 내용이 들어간다. **navigation bar** 와도 유사할 수 있다. **aside** 라는 태그를 사용할 수 있고, 주로 main 요소 내에 배치되는 경우가 많다.

**footer** 영역은 주소, 카피라이트, 연락처 정보 등이 포함된다. header 부분에 비해서는 중요도가 약간은 떨어지는 정보들이 들어간다고 생각하시면 된다. footer라는 태그를 주로 사용한다.

~~~HTML
<header class="header">헤더</header>
<main class="main">
  <div class="hero-section">히어로 영역</div>
  <div class="main-content">메인 콘텐트</div>
  <div class="side-content">사이드 콘텐트</div>
</main>
<footer class="footer">푸터</footer>
~~~

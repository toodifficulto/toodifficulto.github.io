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

# meta 요소
html의 기본구조를 먼저 생성하면 아래와 같다.

~~~html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <link rel="stylesheet" href="./style.css" />
    <title>The Web framework for perfectionists with deadlines | Django</title>
  </head>
  <body></body>
</html>
~~~

# DOCTYPE 선언
문서 최상단에는 문서 타입이 정의되어 있는데 이를 **DOCTYPE 선언** 이라고 부른다. DOCTYPE은 어떤 버전으로 작성되었는지 브라우저에게 알려주는 선언문 같은 것이다. 반드시 문서 내의 최상단에 위치해야 한다. 지금 보이는 DOCTYPE 선언은 HTML5 버전의 DOCTYPE이고 최근에 가장 많이 사용되고 있다. 다른 DOCTYPE을 발견하신다면 구 버전이라고 생각하시면 되겠다. DOCTYPE을 선언하지 않거나 잘못 선언한 경우, 브라우저가 문서를 잘못된 방식으로 해석할 수 있으니 조심해야 한다.

&nbsp;

# html 요소
다음은 <html> 요소다. <html> 요소는 최상위 레벨의 요소로, **루트 요소** 라고도 부른다. HTML 요소에는 lang이라는 attribute가 들어갈 수 있는데 검색엔진이나 브라우저가 참고할 수 있도록 해당 문서가 어떤 언어로 작성되었는지 나타내주는 속성이다. 한국어로 작성된 사이트라면 ko로 수정하시면 된다.

&nbsp;

# head
**<head>** 요소 안에 들어 있는 부분은 꽤나 내용이 많다. 브라우저 화면에 표시되지 않지만, 문서의 기본 설정이나 스타일시트/자바스크립트 파일을 연결하는 역할을 한다. 또, body를 해석하기 전 기본적으로 알아야 할 정보를 다룬다.

&nbsp;

## meta
**<meta>** 태그는 문서의 일반적인 정보와 문자 인코딩을 명시한다. 빈요소이다.


### <meta charset="UTF-8" />
가장 먼저 나오는 **<meta charset="UTF-8" />** 는 문자 집합 및 문자 인코딩 관련 요소이다. 최근의 웹에서는 전 세계 문자를 표현하기 위한 표준으로 제정된 유니코드를 많이 사용하고 있다. UTF-8은 유니코드 중에서도 널리 사용되는 인코딩이다. 이 부분을 지정하지 않으면, 브라우저가 지원하는 언어가 다를 경우 깨질 가능성이 있다,

### <meta name="viewport" content="width=device-width, initial-scale=1.0" />
**<meta name="viewport" content="width=device-width, initial-scale=1.0" />** 는 반응형 웹사이트를 만드는 경우에 선언되어야 하는 내용이다. 브라우저 크기를 조절해보면 브라우저 크기에 따라 요소 배치가 달라지는 것을 볼 수 있는데 이를 반응형 웹사이트라고 한다.


### <meta http-equiv="X-UA-Compatible" content="ie=edge" />

마지막으로 **<meta http-equiv="X-UA-Compatible" content="ie=edge" />** 는 인터넷 익스플로러 브라우저에서의 렌더링 관련이 있는데 익스플로러의 호환성 보기 모드에서 최신 버전의 엔진을 사용하는 브라우저로 렌더링 하라는 의미이다.

&nbsp;
&nbsp;

## title
<title> 요소는 문서의 제목을 나타내주는데 중요한 텍스트를 입력해야 검색 엔진이 페이지를 잘 확인할 수가 있다. 같은 웹사이트라고 하더라도 페이지마다 핵심 키워드를 따로 제공해주는 것이 좋다.


### link
link라고 쓰고 탭 키를 누르면 자동완성 되는데 rel 속성은 relationship, 즉 관계의 약자로 지금 html 문서와는 어떤 관계인지를 써주면 된다. 우리는 CSS 파일을 연결할 것이므로 stylesheet라고 적으면 된다. href 속성은 파일의 위치를 적어주면 된다.

---
layout: post
title: CSS 개요
tags:  [python-web-development]
---

# CSS
CSS는 시각적으로 꾸며주는 역할을 한다. CSS는 구체적으로 어떤 스타일로 요소가 표시 되는 지를 정하는 규격이라고 할 수 있다.

&nbsp;
&nbsp;
&nbsp;

# 사용법

[참고 사이트](https://ofcourse.kr/css-course/CSS-%EC%9E%85%EB%AC%B8)

CSS 내부적으로 사용되는 문법은 동일하며, 어디에 기술하느냐의 차이가 존재한다.
기술하는 방법은 아래의 3가지가 있다.

### 1. HTML 태그의 style 속성을 이용
~~~html
<!-- 1번 방법 -->
<div style="color: red"> this is red text </div>
~~~

&nbsp;


### 2. <style> 태그를 통해 HTML 문서 내부에 기술 (<style> 태그는 주로 <head>태그 내부에 사용합니다)

~~~HTML
<!-- 2번 방법 -->
<html>
<head>
	<style type="text/css">
		.my-text{ color: blue }
	</style>
</head>
<body>
	<div class="my-text">
		this is red blue
	</div>
</body>
</html>
~~~

&nbsp;

### 3. .css 파일로 분리하여 HTML 문서에 연결
CSS를 별도의 파일로 저장 후 HTML 문서 내에서 불러 올 수도 있다.

~~~HTML
@charset "utf-8";

아래는 모든 h1을 바꾸는 방식이다.
h1{
	font-size: 30px;
	font-weight: normal;
	margin: 0;
	margin-bottom: 10px;
}

아래는 progress-bar라는 id를 가지고 있는 속성만 바꾸는 방식이다. id는 고유한 값으로 오직 하나의 element만 가질 수 있다.
#progress-bar{
	margin: 0;
	padding: 0;
	margin-bottom: 15px;
	list-style: none;
}

아래는 common-btn이라는 class를 가진 element들에게만 적용되는 방식이다.
.common-btn{
	padding: 4px 6px;
	background-color: #07C;
	color: white;
	border: none;
	border-radius: 10px;
	text-decoration: none;
}
~~~

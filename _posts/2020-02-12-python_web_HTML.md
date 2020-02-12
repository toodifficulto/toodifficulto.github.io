---
layout: post
title: HTML
tags:  [python-web-development]
---

# 제목 태그

&&h**를 사용해서 제목 태그를 정의한다. 크기는 h1이 가장 크고 h6가 가장 작다.
~~~HTML
<!-- 제목 태그 -->
<h1>Heading level 1</h1>
<h2>Heading level 2</h2>
<h3>Heading level 3</h3>
<h4>Heading level 4</h4>
<h5>Heading level 5</h5>
<h6>Heading level 6</h6>
~~~

![Alt text](/public/post/2020_02_12_HTML/pic1.PNG)

&nbsp;
&nbsp;

# 단락 정의
**p**는 텍스트를 단락으로 정의할때 사용된다. 그리고 **em은** 이탈릭체 **strong** 은 bold체로 사용된다.


~~~HTML
<p>텍스트를 <em>단락</em>으로 정의할 때 사용합니다. 단락은 유사한 주제를 갖는 문자열의 묶음입니다.

단락은 내용을 담는 그릇이므로 빈 여백을 처리하는 용도로는 사용하면 <strong>안 된답니다!</strong></p>
~~~~

![Alt text](/public/post/2020_02_12_HTML/pic2.PNG)

&nbsp;
&nbsp;

# 리스트 정의
**ol** 은 순서가 있는 리스트를 정의할 때 사용된다. **ul** 은 순서가 없는 리스트를 정의할 때 사용된다.

~~~HTML
<!-- ordered list -->
<ol>
  <li>일</li>
  <li>이</li>
  <li>삼</li>
  <li>사
<!--     unordered list -->
    <ul>
      <li>항목</li>
      <li>항목</li>
      <li>항목</li>
    </ul>
  </li>
  <li>오</li>
</ol>
~~~

&nbsp;
&nbsp;

![Alt text](/public/post/2020_02_12_HTML/pic3.PNG)

# 콘텐츠 구조 표현
**dl**, **dt**, **dd** 은 콘텐츠의 구조를 표현하기 위해 사용된다. code는 글씨체가 코드 글씨체로 나온다.

~~~HTML
<dl>
  <dt>HTML</dt>
  <dd>웹페이지에서 콘텐츠의 구조를 표현하기 위해 고안된 텍스트 포맷입니다.</dd>
  <dt>CSS</dt>
  <dd>웹페이지의 스타일링과 디자인을 하는 데 사용되는 스타일시트입니다.</dd>
  <dt>HTTP 메서드</dt>
  <dd>서버가 수행해야 할 동작을 의미합니다. <code>GET</code>, <code>POST</code>, <code>DELETE</code> 등이 있습니다.</dd>
</dl>
~~~

![Alt text](/public/post/2020_02_12_HTML/pic4.PNG)

&nbsp;
&nbsp;

# 하이퍼링크

하이퍼링크는 아래와 같이 사용한다.

~~~HTML
<a href="https://www.naver.com">MDN으로 이동</a>
<a href="https://w3.org">w3.org로 이동</a>
~~~

![Alt text](/public/post/2020_02_12_HTML/pic5.PNG)

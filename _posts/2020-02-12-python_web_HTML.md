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

&nbsp;
&nbsp;

# div 태그
div 요소는 의미를 가지고 있지 않다. 앞에서 배운 내용은 단락, 표제 태그 등은 하나씩 특정한 의미의 콘텐츠를 담는 요소였지만 div는 그렇지 않다. 그래서 중립적인 태그라고도 한다. div는 생성된 요소를 묶는 역할을 하거나, 기존의 의미를 가진 태그로는 만들 수 없는 요소를 만드는 역할을 한다.

~~~html
<div>div 요소는 앞서 배웠던 제목 요소, 단락 요소 등 하나씩 특정한 의미의 콘텐츠를 담는 요소와는 다르게 의미를 가지고 있지 않습니다. 여러 요소를 디자인 상의 이슈 등으로 묶어야 할 때, 구분이 필요하나 다른 적절한 태그가 없을 때 등 사용이 됩니다.</div>
~~~

![Alt text](/public/post/2020_02_12_HTML/pic6.PNG)

디자인 상의 이슈가 있을 때 레이아웃을 만드는 역할을 할 수 있고, 계층 구조를 부여할 수도 있다. 마크업 문서의 가독성을 향상시킬 수 있고, 또 문서 구조를 파악하기에도 편리하다.

(참고로, codepen에는 tidy HTML이라는 기능이 있는데요. HTML이 공백 또는 줄바꿈을 무시하기 때문에, 코드를 작성할 때 이런 식으로 들여쓰기를 통해서 계층구조를 손쉽게 파악할 수 있다.)

class나 id 속성을 결합하면 포함하는 콘텐트에 특정한 의미를 넣거나 디자인 스타일 또한 쉽게 적용할 수 있다.

&nbsp;
&nbsp;
&nbsp;

# span
div와 다르게 줄바꿈을 하지 않는다. div는 블록 요소, span을 인라인 요소라고 부른다.

~~~html
<div><span>span 요소</span>는 div 요소와 유사하지만, div가 블록 요소인 반면, span은 인라인 요소라는 점에서 차이가 있습니다.
</div> -->
~~~

![Alt text](/public/post/2020_02_12_HTML/pic7.PNG)

&nbsp;
&nbsp;
&nbsp;

# span과 div의 차이

블록요소는 다른 블록 요소를 포함할 수 있으며 또 다른 블록 요소 안으로 들어갈 수 있다. 인라인 요소는 화면에 출력할 때 줄이 바뀌지 않고 같은 줄에 등장합니다. 다른 인라인 요소를 포함할 수 있으나, 블록요소를 포함할 수는 없다.

인라인 요소로는 a, em/strong, span 태그 등이 있다. 블록 요소로는 구조를 나타내는 모든 코드들, p나 h1/h2..., ul/ol, li, div 등이 있다.

이렇게 block 과 inline으로 레이아웃을 만든 상태를 노멀 플로우라고 부른다.

하지만 블록 요소와 inline 요소가 위치하는 방식을 CSS를 통해 변경할 수 있다. css의 display 속성을 사용하면 가능하다.

~~~html
display: block
=> 줄바꿈이 일어납니다. 양 옆에 다른 요소를 가질 수 없습니다.
~~~

~~~html
display: inline
=> inline 속성으로 생성한 요소는 새 블록을 생성하지 않고 바로 다른 요소 사이에 배치됩니다.
~~~

~~~html
<div>
  블록 요소는 줄바꿈이 일어납니다.
</div>
<span>인라인 요소는 줄바꿈이 일어나지 않습니다.</span>
<span>인라인 요소는 줄바꿈이 일어나지 않습니다.</span>
<span>인라인 요소는 줄바꿈이 일어나지 않습니다.</span>
<span>인라인 요소는 줄바꿈이 일어나지 않습니다.</span>
~~~

![Alt text](/public/post/2020_02_12_HTML/pic8.PNG)

display 속성에는 그 외에도 inline과 block의 속성을 모두 지니는 inline-block, 아예 화면에서 사라지게 할 수 있는 가시성 관련된 값들(hidden 등), 그리고 최신 레이아웃 기법을 적용할 수 있는 flex나 grid 등을 값으로 가질 수 있다.

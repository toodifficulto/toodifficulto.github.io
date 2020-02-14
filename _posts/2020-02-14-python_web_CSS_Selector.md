---
layout: post
title: CSS Selector
tags:  [python-web-development]
---

# 셀렉터
선택자, 셀렉터는 특정 HTML 요소를 선택해서 스타일을 적용할 수 있도록 한다.  

아래는 클래스 선택자, 태그 선택자 그리고 아이디 선택자의 예시이다.

~~~html
<div class="class-selector">클래스 선택자</div>
<p>태그 선택자</p>
<p id="id-selector">id 선택자</p>
~~~

&nbsp;
&nbsp;
&nbsp;

# 클래스 선택자
클래스 셀렉터는 가장 범용적으로, 그리고 빈번하게 사용된다.

필요한 경우 여러 개의 클래스를 부여해줄 수도 있다. 공백으로 구분이 가능하고 이러면 요소들을 묶어주기에도 편리하다.

~~~html
<div class="class-selector selector">클래스 선택지</div>
<p class='selector'>태그 선택지</p>
<p id='id-selector'>id 선택지</p>
~~~

~~~css
.class-selector{
  color :red;
}
.selector{
  background=color : yellow;
}
~~~

&nbsp;
&nbsp;
&nbsp;

# 태그 선택자
태그 선택자는 어떨 때 사용되냐 하면, 문서 전체의 태그 속성의 값을 바꿀 때 유용하다.

a 태그 색상은 기본값이 빨간색인데 이를 바꾼다거나, ul 태그에 앞에 붙는 동그라미 모양을 바꾸기 위해 list-style 속성을 바꾼다거나 할 때 사용 가능하다. 하지만 태그 선택자를 너무 많이 사용하는 것은 좋지 않은데 유지보수성이 떨어질 수도 있기 때문이다. 그러니 가급적이면 클래스 선택자를 사용하는 것이 좋다.

~~~css
a {
  color: #000;
  text-decoration: none;
}
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
~~~

# id 선택자
ID 선택자는 선택자 앞에 #이 붙는 것이고 id 속성값은 전체 문서에서 딱 하나의 요소에만 사용할 수 있다.

~~~css
p {
  background: lightgrey;
  font-weight: 100;
}
.selector {
  background: lavender;
  font-weight: 400;
}
#id-selector {
  background: orange;
  border: 1px solid #696969;
  padding: 10px;
  font-weight: 900;
}
~~~

어떤 선택자들보다 Id 선택자가 구체적으로 해당 요소를 지칭하기 때문에, ID 선택자에서 속성과 값을 정의해준 경우에는 그 내용이 다른 선택자를 덮어쓰게 된다.

이러한 규칙을 선택자 **적용 우선 순위** 라고 한다. 구체성이 높은 선택자의 스타일이 적용되는 것이다.

태그 선택자보다는 클래스 선택자가, 클래스 선택자보다는 ID 선택자가 더 구체적이라고 할 수 있고 그래서 먼저 적용이 된다.

### **id 선택자 > 클래스 선택자 > 태그 선택자**

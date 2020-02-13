---
layout: post
title: CSS
tags:  [python-web-development]
---

# 텍스트 관련 속성

글꼴을 지정하는 속성은 **font-family** 다. 값이 하나 이상의 속성값을 지정할 수 있는데 에러에 대비하고, 또 해당 글꼴이 없는 경우 사용자 상황에 맞는 글꼴을 사용할 수 있다는 장점이 있다. 글꼴 이름에 띄어쓰기가 포함되어 있다면 따옴표 안에 글꼴 이름을 넣어주어야 하고 공백 없는 글꼴을 따옴표로 감싸면 에러가 일어날 수 있으니 주의해야 한다.

또한, 폰트가 정말 없을 경우를 대비해서 다섯가지 기본 스타일 중 하나를 지정해서, 해당 컴퓨터가 가지고 있는 폰트 중에서 해당 스타일을 사용할 수 있는 **방법(generic-name)** 도 있다. **serif**, **sans-serif**, **monospace** 등이 있다.

container클래스를 선택해서 font-family속성을 주면 모든 문자에 글꼴이 적용된다. 이것을 **상속** 이라고 한다.


많은 CSS 속성이 부모요소에서 자식요소로 상속되고 굉장히 효율적이다. 하나하나 지정할 필요가 없다.

~~~html
.container {
  font-style: italic;
  tont-weight: 900;
  font-family: Arial, Georgia, Times, "Times New Roman", serif;
  font-size: 1.5rem; /* root em */

  /* font: italic 900 1.5rem Arial, Georgia, Times, "Times New Roman", serif; */
   /* 단축표기법으로도 기재 가능 */

}
~~~

### font-size
font-size는 고정값인 px을 적용할 수 있고, em 등 상대값도 적용이 된다.

하나는 **고정된 픽셀값** 으로 요소마다 글자크기를 지정해주는 방식이고 정확한 크기를 지정할 수 있다. 다른 하나는 부모 요소에 글씨크기를 정해두고 그 크기의 **상대값** 을 사용하는 방식이 있다.

em은 부모 엘리먼트의 폰트 크기에 비례한다. 편리하게 글씨 크기를 조절할 수 있지만, 합성 문제가 발생합니다. 예를 들어, 태그 안에 같은 태그가 그려지는 li의 경우를 한번 살펴보면 2em이라고 li에 설정을 해두면, 합성된 아래의 글씨 크기는 2em의 2배, 그러니까 4em이 되고 만다.

~~~html
li {
  font-size: 1.5em;
  color: rgba(0, 0, 0, 0.5);
}
~~~

![Alt text](/public/post/2020_02_13_CSS/pic3.PNG)

rem 값은 부모 엘리먼트가 아닌 최상위 html 엘리먼트에 상대적으로 적용되어 최상단 요소에 적용한 크기를 기준으로 상대값을 표현할 수 있어 이 문제를 해결할 수 있다.

~~~html
html {
  font-size: 10px;
}
.container {
  font-style: italic;
  tont-weight: 900;
  font-family: Arial, Georgia, Times, "Times New Roman", serif;
  font-size: 1.5rem; /* root em */
}

  /* font: italic 900 1.5rem Arial, Georgia, Times, "Times New Roman", serif; */
  /* 단축표기법으로도 기재 가능 */

li {
  font-size: 1.5rem;
  color: rgba(0, 0, 0, 0.5);
}
~~~


![Alt text](/public/post/2020_02_13_CSS/pic4.PNG)

### 기타

**font-weight** : 글자 두께를 지정하는 속성
 bold, normal 같은 키워드로도 가능하고 100~900 사이의 숫자로도 가능하다.

 **font-style** : italic, normal처럼 글씨의 스타일을 나타낸다.

다양한 font 속성은 **단축표기법** 으로도 지정이 가능하다.
규칙이 정해져 있는 경우가 많다.

**font-family** 는 가장 마지막에 있어야 하고, **font-size** 보다 **font-style**, **font-weight** 가 먼저 나와야 한다.

~~~html
font: italic 900 1.5rem Arial, Georgia, Times, "Times New Roman", serif;
~~~

&nbsp;
&nbsp;
&nbsp;

# 텍스트 레이아웃

**text-align** 은 글자 정렬과 관련이 있다. 가운데정렬(center), 왼쪽정렬(left), 오른쪽정렬(right), 양쪽정렬(justify) 등이 가능하다.

**line-height** 는 줄간격이다. 글줄 사이의 간격을 의미한다. 줄간격을 표시할 때는 단위가 없는 숫자만 사용하는 것이 가능하고 가급적 그렇게 하는 것이 좋습니다. 이때 숫자는 font-size의 배율이다. 브라우저에서의 line-height는 기본적으로 1 혹은 1.2이다.

선택한 요소가 포함하고 있는 콘텐츠의 글자 색상을 지정하기 위해서는 **color 속성을** 사용한다. font-color, text-color 등의 속성은 적용이 안된다.

색상값을 나타내는 방법에는 크게 네 가지가 있다.

**1.색상 이름**
120개의 키워드가 지정되어 있어 영어를 그대로 사용하면 된다. 우리가 흔히 아는 red, yellow, blue 같은 색상부터 aliceblue, palegreen, papayawhip 같은 생소한 이름도 있다.

[색상이름 사이트](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value#Color_keywords)

**2.16진수 RGB 값**
RGB 값은 빨강, 초록, 파랑 세 종류의 광원을 혼합해서 색상을 만드는 방식이다.

**3. HSL 값**
색상/채도/명도를 기준으로 색상을 나타내는 HSL 값으로도 가능하다.

배경색의 속성명은 **background-color** 이다. 색상값을 지정하여 사용할 수 있다. 배경이미지를 지정하는 background-image 속성도 적용이 가능하다.


~~~html
p {
  color: #fff;
  background-color: red;
  font-size: 1.5em
  부모 요소를 기준으로 상대적으로 적용
  text-align: center;
  line-height: 1.5rem;
}
~~~

![Alt text](/public/post/2020_02_13_CSS/pic5.PNG)

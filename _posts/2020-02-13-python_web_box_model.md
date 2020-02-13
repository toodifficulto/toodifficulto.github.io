---
layout: post
title: Box Model(박스 모델)
tags:  [python-web-development]
---

# 박스 모델
HTML의 요소들이 페이지에 배치될 때, 모든 요소는 각각 사각형 상자로 표현되는데 이를 CSS 박스 모델이라고 부른다.

개발자 도구를 열어 보면 하단에 보면 겹겹이 싸여 있는 사각형이 있는데 HTML의 모든 요소는 이렇게 박스 모델에 따라서 크기와 위치가 결정된다.

![Alt text](/public/post/2020_02_13_box_model/pic1.PNG)

박스 모델은 4개의 컴포넌트, **마진**, **보더**, **패딩**, **콘텐트** 로 구성되어 있다.

### 콘텐트 영역
콘텐츠 영역은 글, 이미지 등 요소의 실제 내용이 포함되는 부분을 말한다. 콘텐트 영역의 크기를 직접 지정할 수도 있는데 width와 height 속성 등을 사용해서 너비와 높이를 지정할 수 있다.

### padding 영역
padding 영역은 박스 안쪽 여백을 의미한다. 콘텐트 영역과 테두리인 border 사이의 여백을 의미한다. padding-top, padding-right, padding-bottom, padding-right를 지정할 수 있다. 네 방향 모두 지정할 수도 있고, 필요한 부분만 지정할 수도 있다.

### boarder 영역
테두리, border 영역은 padding과 margin 사이 테두리 부분이다. 마찬가지로 상하좌우 적용이 가능하다.

### margin 영역
margin 영역, 바깥 여백 영역은 border를 기준으로 다른 요소와의 바깥쪽 여백을 의미한다. margin 또한 상하좌우에 적용할 수 있다.

&nbsp;
&nbsp;
&nbsp;


# HTML 코드
아래는 html코드와 css코드이다.

~~~HTML
<div class="container">
  <div class="first">첫 번째 박스</div>
  <div class="second">두 번째 박스</div>
  <span class="inline-element">span 태그</span>
  <a href="" class="inline-element">링크 태그</a>
  <img src="https://static.djangoproject.com/img/small-fundraising-heart.d255f6e934e5.png" alt="heart" class="inline-element image-element">
</div>
~~~

~~~CSS
.first{
  box-sizing: content-box;
  background: yellow;
  width: 300px;
  height: 150px;
  border: 5px solid black;
  padding: 50px 70px 20px 100px;
  margin: 30px;
}
.second{
  box-sizing: border-box;
  background: lime;
  width: 300px;
  height: 150px;
  border: 5px dotted white;
  padding: 50px 70px 20px 100px;
  margin: 30px;
}
.inline-element{
  background: aqua;
  width: 200px;
}
~~~

![Alt text](/public/post/2020_02_13_box_model/pic2.PNG)


### width와 height
**width** 와 **height** 는 명시적으로 지정하지 않으면 원래 이미지나 글 등 그 요소의 실제 내용 크기가 적용된다. 명시적으로 지정할 경우에는 px, em/rem, % 등을 사용할 수 있다. 주의할 부분이 있는데 inline 속성인 요소는 width와 height를 지정해도 실제 레이아웃에는 적용되지 않는다. 다만 이미지나 비디오 등 멀티미디어에서는 예외가 있어 속성이 적용 된다.

### padding
**padding** 부분은 배경색 또는 배경 이미지가 적용되는 박스 내부의 영역이다. 반면, margin에는 배경이 지정되지 않는다. 즉, 투명한 영역이라고 생각할 수 있다. padding과 margin 둘 다 상하좌우 적용이 가능한데 단축표기법을 사용해서 위쪽, 오른쪽, 아래쪽, 왼쪽 순서로 지정할 수 있으며 상하/좌우 2개씩 적용도 가능하다.

### boarder
border는 아무런 속성도 부여하지 않으면 기본적으로 어떤 스타일도 적용되지 않는다. border-style, border-width, border-color 을 지정하면 각각 solid, dashed, dotted 등의 스타일, 두께, 색상 등을 지정할 수 있다. 단축표기법을 사용해서 세 가지 속성을 한번에 적을 수도 있다. 위/오/아/왼 각각 또한 지정하는 것도 가능하다.

### margin
margin은 요소 바깥 여백 경계인데 묘한 성질이 있다. 바로 요소 두 개가 서로 맞닿아 마진끼리 닿을 때인데 세로 방향으로 지정한 두 개의 서로 다른 요소가 수직으로 접해있는 경우, 그러니까 margin-bottom과 margin-top이 만나는 경우, 두 요소 사이의 마진 간격은 각각의 margin 크기를 합친 것이 아니고, 두 요소 가운데 큰 margin 수치값을 선택한다. 이를 마진통합/마진상쇄 혹은 margin-collapsing이라고 부른다. 그래서 마진으로 간격을 조절할 때 유의해야 한다.

즉, box1와 box2는 각각 margin이 30px인데 이 둘 사이의 margin이 합쳐서 60이 아니라 더 큰 box1의 margin인 30px로 된다.

![Alt text](/public/post/2020_02_13_box_model/pic3.PNG)

### 요소의 크기
한 요소의 크기는 어떻게 결정 될까?  앞서 width와 height 속성으로 콘텐트의 크기를 정할 수 있다고 하였다. 이렇게 콘텐트의 너비와 높이만으로 width와 height 값을 산정하는 것은 box-sizing 속성이 content-box 값일 때이다. 이게 브라우저 기본 설정이다. 그런데 이렇게 되면 레이아웃을 그릴 때 padding 값과 border 두께를 매번 계산하고 고려해야 한다는 문제가 있다. 그래서 padding과 border 두께까지 포함한 것을 요소의 width와 height로 적용하려면 box-sizing 속성을 border-box로 바꾸면 된다.

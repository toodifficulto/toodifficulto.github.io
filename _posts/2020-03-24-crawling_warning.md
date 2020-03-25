---
layout: post
title: 크롤링 사전 주의사항
tags:  [crawling]
---
# 사전 기초 지식

#### 1. 대상 웹 페이지 조건 확인 - robots.txt

#### 2. 크롤러 분류 - 상태 유무, Javascript 유무

#### 3. Request 요청 주의 할 점 - 서버 부하 고려

#### 4. 콘텐츠 저작권 문제

#### 5. 페이지 구조 변경 가능성 숙지

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 1. 대상 웹 페이지 조건 확인 - robots.txt

http://www.hanbit.co.kr/robots.txt
를 검색해보자. 그러면 다음과 같이 나온다.

User-agent: *
Disallow: /hb_admin/
Disallow: /myhanbit/

/hb_admin/, /myhanbit/ 말고는 허락한다는 의미이다.

좀 더 자세한 내용은 다음 사이트에 있다.

[robots.txt 설명글](https://m.blog.naver.com/PostView.nhn?blogId=http-log&logNo=221104827805&proxyReferer=https%3A%2F%2Fwww.google.com%2F)

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 2. 크롤러 분류 - 상태 유무, Javascript 유무

이 사이트가 일반적인 html 코드로 적혀있는 지 혹은 javascript로 적혀있는 지에 따라 접근 방식이 달라진다. 만약 ajax 비동기식을 사용한 javascript 방식이라면 selenium을 사용해야 한다.

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;


# 3. Request 요청 주의 할 점 - 서버 부하 고려

네이버 사이트나 구글 사이트에서 request 요청을 너무 많이 보내면 서버 부하가 된다. 그래서 타이밍을 고려해서 request 요청을 해야 한다. 빠른 시간안에 for문을 돌려서 request 요청 주의를 하면 ip가 ban당할 수 도 있다.

상대 사이트에 대한 예의를 지켜야 한다. __그래서 충분한 간격을 주고 보내야 한다.__

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 4. 콘텐츠 저작권 문제
크롤링한 이미지나 비디오등은 사업적으로 사용하면 license를 확인해야 한다. + 논문, pdf도 중요하다.

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 5. 페이지 구조 변경 가능성 숙지
자동화 시킨 코드가 있는데 해당 사이트가 코드를 변경한 경우 코드를 바꾸어야 한다.

그래서 **API를 활용해서 데이터를 수집하는 것이 가장 좋은 경우이다.** 

그러나 부득이하게 API가 없다면, robots.txt를 체크하고 서버 부하를 피하기 위해 간격도 고려하여야 한다.

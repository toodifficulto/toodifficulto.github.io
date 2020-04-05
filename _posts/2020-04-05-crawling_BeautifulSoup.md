---
layout: post
title: BeautifulSoup실습
tags: [crawling]
---

# BeautifulSoup이란?
BeautifulSoup은 HTML과 XML files에서 data를 빼오는 파이썬 라이브러리이다.

BeautifulSoup을 실습해보자. 다음과 같은 html 코드가 있다고 해보자.
~~~python
from bs4 import BeautifulSoup

html = """
<html>
    <head>
        <title>The Dormouse's story</title>
    </head>
    <body>
        <h1>This is h1 area</h1>
        <h2>This is h2 area</h2>
        <p class="title"><b>The Dormouse's story</b></p>
        <p class="story">Once upon a time there were three little sisters.
            <a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>
            <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a>
            <a data-io="link3" href="http://example.com/little" class="sister" id="link3">Title</a>
        </p>
        <p class='story'>
            story...
        </p>
    </body>
</html>
"""
~~~
그리고 BeautifulSoup를 사용하려면 다음과 같다. 일반적으론 html파일인 requests.get(url).text와 html.parser를 입력받는다.

~~~python
# 예제 1
# soup = BeautifulSoup(requests.get(url).text)
soup = BeautifulSoup(html, 'html.parser')

# 타입확인
print("soup : ", type(soup))
print("prettify : ", soup.prettify())
~~~

prettify는 입력받은 html 코드를 예쁘게 출력해주는 함수이다.
~~~
soup :  <class 'bs4.BeautifulSoup'>
prettify :  <html>     
 <head>
  <title>
   The Dormouse's story
  </title>
 </head>
 <body>
  <h1>
   This is h1 area     
  </h1>
  <h2>
   This is h2 area
  </h2>
  <p class="title">
   <b>
    The Dormouse's story
   </b>
  </p>
  <p class="story">
   Once upon a time there were three little sisters.
   <a class="sister" href="http://example.com/elsie" id="link1">
    Elsie
   </a>
   <a class="sister" href="http://example.com/lacie" id="link2">
    Lacie
   </a>
   <a class="sister" data-io="link3" href="http://example.com/little" id="link3">
    Title
   </a>
  </p>
  <p class="story">
   story...
  </p>
 </body>
</html>
~~~
&nbsp;
&nbsp;
&nbsp;

# 태그 접근
태그를 접근하려면 soup.html.body.p 혹은 .h1등으로 접근 할 수 있다.
~~~python
# h1 태그 접근
h1 = soup.html.body.h1
print('h1 ', h1)

# p 태그 접근
print("p", p)
p = soup.html.body.p
~~~

~~~
h1  <h1>This is h1 area</h1>
p <p class="title"><b>The Dormouse's story</b></p>
~~~

텍스트만 출력하려면 .text나 .string을 사용하면 된다.
~~~python
# 텍스트 출력 1
print('h1 >>', h1.string)

# 텍스트 출력 2
print('p >>', p.string)
~~~

~~~
h1 >> This is h1 area
p >> The Dormouse's story
~~~

&nbsp;
&nbsp;
&nbsp;

# Find, Find_all, Select, Select_one
BeautifulSoup가 element를 접근하는 방식은 크게
**CSS 선택자**, **태그방식** 2가지가 있다.

| 방식 | Method |
|---|:---:|
| `CSS 선택자` | Select, Select_one|
| `태그로 접근` | Find, Find_all |

### Find_all, Find방식으로 찾기.
Find_all은 조건에 맞는 모든 Tag를 찾는다.

~~~python
# 클래스 이름으로 찾기
link2 = soup.find_all('a', class_='sister')
print("link2 : ", link2)

# 실제 값으로 찾기
link3 = soup.find_all('a', string=['Elsie'])
print("link3 : ", link3)

link4 = soup.find_all('a', string=['Elsie', 'Title'])
print("link4 : ", link4)
~~~

~~~
link2 :  [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, <a class="sister" data-io="link3" href="http://example.com/little" id="link3">Title</a>]
link3 :  [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]
link4 :  [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>, <a class="sister" data-io="link3" href="http://example.com/little" id="link3">Title</a>]
~~~

반면에 find는 가장 처음에 발견한 태그만 반환한다.

~~~python
# 처음 발견한 a 태그 선택
link3 = soup.find('a')
print(link3)
~~~

~~~
<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>
~~~

다음과 같이 다중 조건으로 찾을 수도 있다.
~~~python
# 다중 조건
link4 = soup.find('a', {"class" : "sister", "data-io" : "link3"})
print(link4)
~~~

~~~
<a class="sister" data-io="link3" href="http://example.com/little" id="link3">Title</a>
~~~

### CSS 선택자 : select, Select_one
Tag명 대신 CSS선택자로도 접근할 수 있다. **select** 는 find_all과 같고 **select_one** 은 find와 같다.
참고로, CSS가 좀 더 세부적으로 내용을 가지고 올 수 있다.

~~~python
link5 = soup.select_one("p.title > b")
print("link5 : ", link5)

link6 = soup.select_one("a#link1")
print("link6 : ", link6)
~~~

~~~
link5 = soup.select_one("p.title > b")
print("link5 : ", link5)

link6 = soup.select_one("a#link1")
print("link6 : ", link6)

link7 = soup.select_one("a[data-io='link3']")
print("link7 : ", link7)

link8 = soup.select("p.story > a:nth-of-type(2)")
print("link 8 : ", link8)
~~~

~~~
link5 :  <b>The Dormouse's story</b>
link6 :  <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>
link7 :  <a class="sister" data-io="link3" href="http://example.com/little" id="link3">Title</a>
link 8 :  [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
~~~

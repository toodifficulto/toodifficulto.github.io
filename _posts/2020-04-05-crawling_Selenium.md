---
layout: post
title: Selenium 활용하기
tags: [crawling]
---
# Selenium이란
selenium은 웹 어플리케이션을 테스트하기 위해 사용되는 오픈소스 자동화 프레임워크이다. Java, C#, Python등 여러 프로그래밍 언어에서 제공을 한다.

Ajax(Asynchronous Javascript And Xml)를 사용해 데이터를 제공하는 경우 requests나 BeautifulSoup으론 사용하지 못한다. 그래서 이런 경우엔 __Selenium__ 을 사용한다.

&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 설치방법
먼저 selenium을 설치해준다.

~~~
pip install selenium
~~~

Selenium은 **webdriver** 라는 것을 통해 디바이스에 설치된 브라우저들을 제어할 수 있다. Chrome Driver를 아래 사이트에서 다운받아보자.

[Chrome Web Driver 설치](https://sites.google.com/a/chromium.org/chromedriver/downloads)

&nbsp;
&nbsp;
&nbsp;

# Selenium 활용법
먼저 selenium을 import해주고 __webdriver__ 를 설정한다. (Chrome, Firefox 등)

webdriver.Chrome()에는 chrome Driver가 설치되어 있는 경로를 인자값으로 넘긴다.
~~~python
# selenium 임포트
from selenium import webdriver

# webdriver 설정(chrome,  FireFox 등)
browser = webdriver.Chrome('C:/Users/ASUS/Pictures/Desktop/chromedriver.exe')
~~~

위 코드를 실행하면 빈화면의 chrome이 실행된다.

![Alt text](/public/post/2020_04_11_Selenium/chromeDriver.PNG)

이제 browser에 내부 대기를 하고 브라우저의 사이즈를 세팅한 다음 **daum** 홈페이지로 이동해보자.

~~~python
# 내부 대기
browser.implicitly_wait(5)

# 브라우저 사이즈
browser.set_window_size(1920, 1280) # maximize_window(), minimize_window()

# 페이지 이동
browser.get('https://www.daum.net')
~~~
chromeDriver가 daum 홈페이지로 연결되었다.
![Alt text](/public/post/2020_04_11_Selenium/daum.PNG)

&nbsp;

그리고 세션 값, 타이틀, url 그리고 쿠키 정보를 출력해보자.

~~~python
# 세션 값
print("Session ID : {}".format(browser.session_id))

# 타이틀 출력
print("Title : {}".format(browser.title))

# 현재 URL 출력
print("URL : {}".format(browser.current_url))

# 현재 쿠키 정보 출력
print("Cookies : {}".format(browser.get_cookies()))
~~~

그러면 결과값은 다음과 같이 출력된다.
~~~
Session ID : 9e1c6d83ebf93757bf902871cdf18265
Title : Daum
URL : https://www.daum.net/
Cookies : [{'domain': '.daum.net', 'expiry': 1901953636.803852, 'httpOnly': False, 'name': 'webid', 'path': '/', 'sameSite': 'None',
'secure': True, 'value': 'c26a8f061cca4a4989be357ef6df532b'}, {'domain': '.www.daum.net', 'httpOnly': False, 'name': 'dtck_blog', 'path': '/', 'secure': False, 'value': '-1'}, {'domain': '.www.daum.net', 'httpOnly': False, 'name': 'dtck_channel', 'path': '/', 'secure': False, 'value': '-1'}, {'domain': '.daum.net', 'expiry': 1901953636.8039, 'httpOnly': False, 'name': 'webid_sync', 'path': '/', 'sameSite': 'None', 'secure': True, 'value': '1586593634569'}, {'domain': '.www.daum.net', 'httpOnly': False, 'name': 'dtck_media', 'path': '/', 'secure': False, 'value': '-1'}, {'domain': '.daum.net', 'expiry': 2208673636.149747, 'httpOnly': True, 'name': 'TIARA', 'path': '/', 'secure': False, 'value': 'zl5O4Ety-6invHVI7Ta.mV3i46KoCvK9q4Jl3qPhZVnzSB9XEEKNXcI-p5D6_hGRw-SPWxnpvZyIMA8.rgqk4bBauxdJ34hI'}, {'domain': '.www.daum.net', 'httpOnly': False, 'name': 'dtmulti', 'path': '/', 'secure': False, 'value': '-1'}, {'domain': '.www.daum.net', 'httpOnly': False, 'name': 'dtck_refresh', 'path': '/', 'secure': False, 'value': '0'}, {'domain': '.www.daum.net', 'httpOnly': False, 'name': 'dtck_photo_slide', 'path': '/', 'secure': False, 'value': '-1'}, {'domain': '.www.daum.net', 'expiry': 1679905635, 'httpOnly': False, 'name': 'dtck_first', 'path': '/', 'secure': False, 'value': '20200411'}]
~~~

이제 검색창 input을 선택하고 __방탄소년단__ 을 검색해보자.

~~~python
#검색창 input 선택
element = browser.find_element_by_css_selector("div.inner_search > input.tf_keyword")

# 검색어 입력
element.send_keys("방탄소년단")

# 검색 (Form Submit)
element.submit()

# browser.quit()
~~~

다음 사이트에 잘 들어간 것을 볼 수 있다.

![Alt text](/public/post/2020_04_11_Selenium/daum.PNG)

&nbsp;
&nbsp;
&nbsp;

### implicitly_wait이란?
Selenium에서 브라우저 자체가 웹 요소들을 기다리도록 만들어주는 옵션이다. 그래서 .get()으로 지정해준 URL을 가져올 때 각 HTML요소(Element)가 나타날 때 까지 최대 5초까지 기다려준다.

그리고 **find_element_by_css_selector와** 같은 방식으로 HTML엘리먼트를 찾을 때 만약 요소가 없다면 요소가 없다는 **No Such Element와** 같은 Exception을 발생시키기 전 모든 시도에서 5초를 기다린다.

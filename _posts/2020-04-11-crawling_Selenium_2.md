---
layout: post
title: Selenium 활용하기 (2)
tags: [crawling]
---

# Explicitly Wait과 Implicitly Wait

Selenium WebDriver를 이용해 실제 브라우저를 동작시켜 크롤링을 진행하다면 가끔 **NoSuchElementException** 에러가 나는 경우를 볼 수 있다.

가장 대표적인 사례가 JS를 통해 동적으로 HTML구조가 변하는 경우인데, 만약 사이트를 로딩한 직후에(JS처리가 끝나지 않은 상태에서) JS로 그려지는 HTML 엘리먼트를 가져오려고 하는 경우가 있다.

그래서 크롤링 코드를 작성 할 때 크게 두 가지 방법으로 브라우저가 HTML Element를 기다리도록 만들어 줄 수 있다.

&nbsp;
&nbsp;
&nbsp;

# Implicitly Wait
[전 글](https://toodifficulto.github.io/2020/04/11/crawling_Selenium/)에서 설명했듯이 Selenium에서 브라우저 자체가 웹 요소들을 기다리도록 만들어주는 옵션이 **Implicitly Wait** 이다.

하지만 이런 방식은 크롤링하려는 웹이 ajax를 통해 HTML 구조를 동적으로 바꾸고 있다면 과연 '5'초가 적절한 값일지에 대해 고민을 하게 된다. (모든 ajax가 5초안에 끝날 일 이 없다.)

> NOTE: 기본적으로 Implicitly wait의 값은 0초초이다. 즉, 요소를 찾는 코드를 실행시킨 때 요소가 없다면 전혀 기다리지 않고 Exception을 raise하는 것이다.

그래서 조금 더 발전된 기다리는 방식인 **Explicitly Wait** 이 있다.

&nbsp;
&nbsp;
&nbsp;
&nbsp;

# Explicitly Wait
인터넷 웹 사이트를 크롤링하는데 ajax를 통해 HTML구조가 변화는 상황이고, 각 요소가 들어오는 시간은 몇 초가 될지 예상할 수 없는 상황이라고 하자.

위에서 설정한 implicitly_wait을 이용하면 어떤 특정한 상황으로 인해 느려진 경우 평소에 기대하던 5초를 넘어간 경우 Exception이 발생한다. 따라서 명확하게 특정 Element가 나타날때까지 기다려주는 **Explicitly_Wait** 을 사용할 수 있다!

**먼저 필요한 라이브러리를 import해보자**

~~~python
from selenium import webdriver
import time
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.chrome.options import Options
from bs4 import BeautifulSoup
~~~

webdriver가 나오지 않게 option을 추가할 수 도 있다.

~~~python
chrome_options = Options()
chrome_options.add_argument('--headless')

browser = webdriver.Chrome('C:/Users/ASUS/Pictures/Desktop/chromedriver.exe', options=chrome_options)
~~~

그러면 webdriver가 켜지지 않고 실행이 된다.

그리고 나선 크롬 브라우저 내부 대기를 하고, danawa 사이트로 이동해보자.

~~~python
# 크롬 브라우저 내부 대기
browser.implicitly_wait(5)

# 페이지 이동
browser.get("http://prod.danawa.com/list/?cate=112758&15main_11_02")
~~~

그리고 나선 노트북 제조사별에서 +21버튼을 누르고 apple을 눌러보자.

![Alt text](/public/post/2020_04_11_Selenium/company_1.PNG)

![Alt text](/public/post/2020_04_11_Selenium/company_2.PNG)

이때 Explicitly_Wait을 사용해보자.

아래 코드를 사용해서 찾으려는 대상을 driver가 명시적으로 '2초'와 '3초'를 기다리도록 만들 수 있다. 
~~~python
WebDriverWait(browser, 3).until(EC.presence_of_element_located((By.XPATH, '//*[@id="dlMaker_simple"]/dd/div[2]/button[1]'))).click()
WebDriverWait(browser, 2).until(EC.presence_of_element_located((By.XPATH, '//*[@id="selectMaker_simple_priceCompare_A"]/li[13]/label'
))).click()
~~~

---
layout: post
title: Requests 실습 (1) Session 및 Cookies 사용
tags:  [crawling]
---

# Cookies란?
쿠키란 **하이퍼 텍스트의 기록서(HTTP)의 일종으로서 인터넷 사용자가 어떠한 웹사이트를 방문할 경우 그 사이트가 사용하고 있는 서버를 통해 인터넷 사용자의 컴퓨터에 설치되는 작은 기록 정보 파일** 을 일컫는다. 이 기록 파일에 담긴 정보는 인터넷 사용자가 같은 웹사이트를 방문할 때마다 읽히고 수시로 새로운 정보로 바뀐다.

[위키피디아 펌](https://ko.wikipedia.org/wiki/HTTP_%EC%BF%A0%ED%82%A4)

&nbsp;
&nbsp;

# Requests란?
requests는 http와 관련된 내용이나 API를 사용할 때 활용되는 모듈
이다.

### Session이란
쿠키와 마찬가지로 클라이언트와 서버의 연결을 유지시켜주는 방법 중 하나이다. http프로토콜은 요청(클라이언트 -> 서버) 한 번과, 응답(서버 -> 클라이언트) 한 번이 이루어지면, 연결을 해제한다. **연결을 계속 유지 시 서버 과부하가 걸릴 수 있기 때문이다.** 그래서 기존 정보를 계속 유지할 방법이 필요하다.

**요청과 응답이 이루어지고 나면 session을 사용하여 해당 정보를 저장하고 있는다.**

**쿠키와 달리, 세션은 웹 컨테이너 ,즉 서버에서 만들어진다.**

####이 Requests 모듈의 Session 을 사용해서 cookies값을 전송해보자!

먼저 requests모듈의 Session 인스턴스를 만들어주자. 그리고 나서 get() 방식을 통해 naver에 요청을 한다. 그리고 나서 출력 값을 보면 정상적으로 수신이 된 것을 볼 수 있다.

~~~python
# Session 및 쿠키 사용
import requests

# 세션 활성화
s = requests.Session()
r = s.get('http://www.naver.com')

# 수신 데이터 출력
print(r.text)
~~~

수신이 잘 되었는 지 체크하는 방법은 아래와 같다.

~~~python

# 수신 상태 코드
print("Status Code : {}".format(r.status_code))
print("Ok ? ".format(r.ok))
~~~

~~~
Status Code : 200
Ok True
~~~

&nbsp;
&nbsp;
&nbsp;
&nbsp;

# cookies를 함께 요청하기.
이제 Session을 사용해서 cookies와 함께 get방식으로 요청을 해보자.

테스트를 진행하기 위해 [httpbin 사이트](https://httpbin.org/) 를 사용할 예정이다. 이 사이트는 HTTP request와 response를 테스트해 볼 수 있는 사이트이다.

이 사이트에 들어가서 cookies를 클릭해보면 아래와 같다.
![Alt text](/public/post/2020_03_31_cookies/bin_cookies.PNG)

그리고 나서 GET방식을 클릭해보면 어떻게 해야 하는 지에 대한 설명이 있다. 이를 참고하여 코드를 다음처럼 만들어보자.

Session을 만들고 난 후 get방식으로 요청을 한다. 이 때 cookies값을 dictionary형태로 함께 요청한다.
~~~python
s = requests.Session()
r = s.get("https://httpbin.org/cookies", cookies = {"name" : "kim2"})

# 결과 출력
print(r.headers)
print(r.text)
~~~

그리고 나서 결과값을 보면 다음과 같이 잘 요청 된 것을 볼 수 있다.
~~~
{'Date': 'Tue, 31 Mar 2020 12:30:36 GMT', 'Content-Type': 'application/json', 'Content-Length': '42', 'Connection': 'keep-alive', 'Server': 'gunicorn/19.9.0', 'Access-Control-Allow-Origin': '*', 'Access-Control-Allow-Credentials': 'true'}
{
  "cookies": {
    "name": "kim2"
  }
}
~~~

이제 headers값과 cookies값을 같이 주자.
~~~python
# Headers값 함께 주기
url = 'https://httpbin.org/cookies'
headers = {'user-agent' : 'nice_man_try_chrome'}

# Headers 정보 전송
r = s.get(url, headers = headers, cookies = {'name' : 'bae'})

print(r.text)
print(r.headers)
~~~

그러면 다음과 같이 결과값이 나온다.

~~~

  "cookies": {
    "name": "bae"
  }
}

{'Date': 'Tue, 31 Mar 2020 12:33:22 GMT', 'Content-Type': 'application/json', 'Content-Length': '41', 'Connection': 'keep-alive', 'Server': 'gunicorn/19.9.0', 'Access-Control-Allow-Origin': '*', 'Access-Control-Allow-Credentials': 'true'}
~~~

&nbsp;
&nbsp;
&nbsp;

# with문과 함께 사용
보통 Session을 사용하게 되면 with문과 함께 사용해서 close를 꼭 해주어야 한다!

~~~python
with requests.Session() as s:
    r = s.get('https://daum.net')
    if r.ok:
        print(r.text)
~~~

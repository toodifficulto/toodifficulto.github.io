---
layout: post
title: Requests 실습 (3) - REST API
tags: [crawling]
---
# REST API란
REST API에 대한 설명은 [REST API 설명](https://toodifficulto.github.io/2020/03/31/crawling_rest_api/)에 적어놓았다. 이제 이 REST API를 Requests를 사용해서 실습해보자.

먼저 REST API는 **GET, POST, DELETE, PUT** 을 사용한다. 그리고 URL을 활용해서 자원의 상태 정보를 주고 받는 모든 것을 의미한다.

먼저 **쿠키** 를 requestsCookieJar를 사용해서 설정해준다.
그런 다음 **GET방식으로** **httpbin.org/cookies에** 요청을 보낸다.  

~~~python
import requests
import json

s = requests.Session()

# 쿠키 설정
jar = requests.cookies.RequestsCookieJar()

# 쿠키 삽입
jar.set("name", "niceman", domain = 'httpbin.org', path = '/cookies')

# 요청
r = s.get("http://httpbin.org/cookies", cookies = jar)

# 출력
print(r.text)
~~~

그러면 다음과 같이 response가 온다.
~~~
{
  "cookies": {
    "name": "niceman"
  }
}
~~~


이제 **POST방식을** 사용해보자.
~~~python

r = s.post('https://httpbin.org/post', data = {"id" : 'test77', "pw" : "111"}, cookies = jar)

print(r.text)
print(r.headers)
~~~
그러면 다음과 같은 결과값이 나온다.
~~~
{
  "args": {},
  "data": "",
  "files": {},
  "form": {
    "id": "test77",
    "pw": "111"
  },
  "headers": {
    "Accept": "*/*",
    "Accept-Encoding": "gzip, deflate",
    "Content-Length": "16",
    "Content-Type": "application/x-www-form-urlencoded",
    "Host": "httpbin.org",
    "User-Agent": "python-requests/2.23.0",
    "X-Amzn-Trace-Id": "Root=1-5e8975e0-11dc0378d9ec789cc67202e0"
  },
  "json": null,
  "origin": "175.196.24.135",
  "url": "https://httpbin.org/post"
}

{'Date': 'Sun, 05 Apr 2020 06:08:32 GMT', 'Content-Type': 'application/json', 'Content-Length': '499', 'Connection': 'keep-alive', 'Server': 'gunicorn/19.9.0', 'Access-Control-Allow-Origin': '*', 'Access-Control-Allow-Credentials': 'true'}
~~~

data를 넘겨 줄 때 dictionary형태와 tuple형태 두 가지 방식으로 줄 수 있다.

~~~python
url = 'https://httpbin.org/post'

payload1 = {"id" : "test77", "pw" : '111'}
payload2 = (('id', 'test77777'), ('pw', '11111'))

r1 = s.post(url, data= payload1)
r2 = s.post(url, data= payload2)

print(r1.text)
print("*" * 100)
print(r2.text)
~~~

~~~
{
  "args": {},
  "data": "",
  "files": {},
  "form": {
    "id": "test77",
    "pw": "111"
  },
  "headers": {
    "Accept": "*/*",
    "Accept-Encoding": "gzip, deflate",
    "Content-Length": "16",
    "Content-Type": "application/x-www-form-urlencoded",
    "Host": "httpbin.org",
    "User-Agent": "python-requests/2.23.0",
    "X-Amzn-Trace-Id": "Root=1-5e8976cc-cb4d9360f6316ef481f12c40"
  },
  "json": null,
  "origin": "175.196.24.135",
  "url": "https://httpbin.org/post"
}

****************************************************************************************************
{
  "args": {},
  "data": "",
  "files": {},
  "form": {
    "id": "test77777",
    "pw": "11111"
  },
  "headers": {
    "Accept": "*/*",
    "Accept-Encoding": "gzip, deflate",
    "Content-Length": "21",
    "Content-Type": "application/x-www-form-urlencoded",
    "Host": "httpbin.org",
    "User-Agent": "python-requests/2.23.0",
    "X-Amzn-Trace-Id": "Root=1-5e8976cc-8f6aa70871a56bc0b16a3848"
  },
  "json": null,
  "origin": "175.196.24.135",
  "url": "https://httpbin.org/post"
}
~~~

이제 **PUT** 을 실습해보자. PUT은 수정하기 위해서 주로 사용하는 API이다.

~~~python
# 예제 (PUT)
url = 'https://httpbin.org/put'
r = s.put(url, data={"id" : '1'})
print(r.text)
~~~
결과값은 다음과 같다.
~~~
{
  "args": {},  
  "data": "",  
  "files": {},
  "form": {    
    "id": "1"
  },
  "headers": {
    "Accept": "*/*",
    "Accept-Encoding": "gzip, deflate",
    "Content-Length": "4",
    "Content-Type": "application/x-www-form-urlencoded",
    "Host": "httpbin.org",
    "User-Agent": "python-requests/2.23.0",
    "X-Amzn-Trace-Id": "Root=1-5e897764-f40aabea704abee6ab47d5e1"
  },
  "json": null,
  "origin": "175.196.24.135",
  "url": "https://httpbin.org/put"
}
~~~
이제 마지막으로 **DELETE** 를 실습해보자.
~~~python
# 예제 (DELETE)
url = 'https://httpbin.org/delete'
r = s.delete(url)
print(r.text)
~~~

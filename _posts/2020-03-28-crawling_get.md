---
layout: post
title: get 방식 데이터 통신
tags:  [crawling]
---
# HTTP란
http란 웹상에서 클라이언트와 서버 간에 요청응답으로 데이터를 주고 받을 수 있는 프로토콜이다. 클라이언트가 HTTP 프로토콜을 통해 서버에게 요청을 보내면 서버는 요청에 맞는 응답을 클라이언트에게 전송한다. 이 때 HTTP 요청에 포함되는 **HTTP메소드는 서버가 요청을 수행하기 위해 해야할 행동을 표시하는 용도로 사용된다.** **이 HTTP 메소드 중 GET과 POST가 대표적이다.**

&nbsp;
&nbsp;
&nbsp;


# GET방식
GET은 **서버로부터 정보를 조회하기 위해** 설계된 메소드이다. Get은 요청을 전송할 때 필요한 데이터를 Body에 담지 않고 **쿼리스트링을 통해 전송한다.** URL 끝에 **?** 와 함께 이름과 값으로 쌍을 이루는 요청 파라미터를 쿼리스트링이라고 부르는 데 요청 파라미터가 여러 개이면 **&** 로 연결한다.

&nbsp;
&nbsp;
&nbsp;

# POST 방식
POST는 **리소스를 생성/변경하기 위해 설계** 되었기 때문에 GET과 달리 전송해야할 데이터를 HTTP Body에 담아서 전송한다. HTTP 메시지의 Body는 길이의 제한없이 데이터를 전송 할 수 있다. **그래서 POST 욫청은 GET과 달리 대용량 데이터를 전송할 수 있다.** 그래서 안전하다고 생각 할 수 있지만 크롬 개발자등으로 확인 할 수 있기 때문에 민감한 데이터의 경우 반드시 암호화해야 한다.


&nbsp;
&nbsp;
&nbsp;


# GET과 POST방식의 차이
GET은 Idempotent, POST는 Non-idempotent 하게 설계되었다.

> Idempotent(멱등)은 연산을 여러 번 적용하더라도 결과가 달라지지 않는 성질이다.

멱등이라는 것인 **동일한 연산을 여러 번 수행하더라도 동일한 결과** 가 나타나는 것이다.

**GET은 Idempotent하게 설계되었기 때문에 서버에게 동일한 요청을 여러번 전송하더라도 동일한 응답이 돌아와야 한다.** 그래서 GET은 주로 **조회를 할 때 사용** 한다.

**POST는 Non-Idempotent 하기 때문에 서버에게 동일한 요청을 여러 번 전송해도 응답은 항상 다를 수 있다.** 이에 따라 POST는 서버의 상태나 데이터를 변경 시킬 때 사용된다.

&nbsp;
&nbsp;
&nbsp;
&nbsp;

# GET 사용법
ipify는 GET방식을 테스트하기 좋은 사이트이다.

[ipify사이트](https://www.ipify.org/)

encar라는 중고 자동차 거래 사이트에 requests를 해보자. urlopen한 응답을 geturl, status, getheaders, getcode함수등을 통해 체크할 수 있다.

그리고 아래에 보이는
http://www.encar.co.kr?test=test 가 대표적인 GET방식의 요청이다 kr뒤에 ?가 있고 test=test라고 적혀있는데 test파라미터에 test라는 값을 넣어서 서버에 전송한다라는 의미이다.


~~~python
import urllib.request
from urllib.parse import urlparse

# 기본 요청 1(encar)

url = 'http://www.encar.com/'

mem = urllib.request.urlopen(url)

# 여러 정보
print('type : {}'.format(type(mem)))
print('geturl : {}'.format(mem.geturl()))
print('status : {}'.format(mem.status))
print("headers : {}".format(mem.getheaders()))
print('getcode : {}'.format(mem.getcode()))
print('read : {}'.format(mem.read(100).decode('utf-8')))
print("parse : {}".format(urlparse('http://www.encar.co.kr?test=test').query))

# Get 방식 : 주요 내용이 url에 노출이 된다. 로그인이나 중요한 원문 데이터들은
# Post 방식을 활용한다.

~~~

이제 ipify에 요청을 해보자. 그리고 **urllib.parse.urlencode** 함수를 통해 GET방식으로 encode해준다. 

~~~python
# 기본 요청2(ipify)
API = 'https://api.ipify.org'

먼저 GET방식으로 보낼 값을 values라는 dictionary에 담는다.

~~~python
# Get방식 Parameter
values = {
    'format' : 'jsonp' #text , jsonp
}

print('before param : {}'.format(values))

params = urllib.parse.urlencode(values)

# dictionary형태를 url encode해준다.
print('after param : {}'.format(params))

# 요청 URL 생성
URL = API + "?" + params
print("요청 URL = {}".format(URL))

# 수신 데이터 읽기
data = urllib.request.urlopen(URL).read()

# 수신 데이터 디코딩
text = data.decode('UTF-8')
print('response : {}'.format(text))

~~~

---
layout: post
title: Requests 실습 (2) JSON 사용법
tags:  [crawling]
---

# JSON(Javascript Object Notation)이란?

* JavaScript Object Notation라는 의미의 축약어로 데이터를 저장하거나 전송할 때 많이 사용되는 **경량의 DATA 교환 형식** 이다.

* Javascript에서 객체를 만들 때 사용하는 표현식을 의미한다.

* JSON 표현식은 사람과 기계 모두 이해하기 쉬우며 용량이 작아서, 최근에는 JSON이 XML을 대체해서 데이터 전송 등에 많이 사용한다.

* JSON은 데이터 포맷일 뿐이며 어떠한 통신 방법도, 프로그래밍 문법도 아닌 단순히 데이터를 표시하는 표현 방법일 뿐이다.

[출저 사이트](https://velog.io/@surim014/JSON%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)

requests와 json은 함께 같이 많이 사용된다.

다음 코드를 통해 실습을 해보자.

httpbin 사이트의 Dynamic data아래의 GET방식의 stream항목을 보면 stream nJSON responses를 송신한다고 적혀있다.

그러면 아래와 같이 httpbin에 GET방식으로 요청을 해보자.
~~~python
# httpbin에 요청하기
url = 'https://httpbin.org/stream/10'

s = requests.Session()

# 10개 데이터 JSON 요청
r = s.get(url, stream=True)

# 수신 확인
print(r.text)
~~~

그러면 다음과 같이 **JSON파일 형태로** 서버가 응답을 보내준다.
~~~
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833ea3-79ed33528a8ea79927374272", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 0}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833ea3-79ed33528a8ea79927374272", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 1}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833ea3-79ed33528a8ea79927374272", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 2}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833ea3-79ed33528a8ea79927374272", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 3}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833ea3-79ed33528a8ea79927374272", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 4}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833ea3-79ed33528a8ea79927374272", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 5}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833ea3-79ed33528a8ea79927374272", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 6}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833ea3-79ed33528a8ea79927374272", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 7}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833ea3-79ed33528a8ea79927374272", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 8}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833ea3-79ed33528a8ea79927374272", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 9}
~~~

그러면 이제 각 송신 값마다 JSON형태로 변환 후 TYPE을 알아보자.

~~~python
for line in r.iter_lines(decode_unicode=True):
    b = json.loads(line)
    print(type(b))
~~~

dictionary형태로 잘 저장된 것을 알 수 있다.

~~~
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f2f-95b38cf4d53fdedc496a30f4", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 0}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f2f-95b38cf4d53fdedc496a30f4", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 1}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f2f-95b38cf4d53fdedc496a30f4", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 2}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f2f-95b38cf4d53fdedc496a30f4", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 3}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f2f-95b38cf4d53fdedc496a30f4", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 4}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f2f-95b38cf4d53fdedc496a30f4", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 5}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f2f-95b38cf4d53fdedc496a30f4", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 6}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f2f-95b38cf4d53fdedc496a30f4", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 7}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f2f-95b38cf4d53fdedc496a30f4", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 8}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f2f-95b38cf4d53fdedc496a30f4", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 9}

<class 'dict'>
<class 'dict'>
<class 'dict'>
<class 'dict'>
<class 'dict'>
<class 'dict'>
<class 'dict'>
<class 'dict'>
<class 'dict'>
(python_crawl) PS C:\Users\ASUS\Programming\Python\web_programming\python_crawl> & C:/Users/ASUS/Programming/Python/web_programming/python_crawl/Scripts/python.exe "c:/Users/ASUS/Programming/Python/web_programming/python_crawl/section04_2_prac .py"
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 0}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 1}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 2}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 3}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 4}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 5}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 6}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 7}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 8}
{"url": "https://httpbin.org/stream/10", "args": {}, "headers": {"Host": "httpbin.org", "X-Amzn-Trace-Id": "Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0", "User-Agent": "python-requests/2.23.0", "Accept-Encoding": "gzip, deflate", "Accept": "*/*"}, "origin": "122.44.107.152", "id": 9}

<class 'dict'>
{'url': 'https://httpbin.org/stream/10', 'args': {}, 'headers': {'Host': 'httpbin.org', 'X-Amzn-Trace-Id': 'Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0', 'User-Agent': 'python-requests/2.23.0', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*'}, 'origin': '122.44.107.152', 'id': 0}
<class 'dict'>
{'url': 'https://httpbin.org/stream/10', 'args': {}, 'headers': {'Host': 'httpbin.org', 'X-Amzn-Trace-Id': 'Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0', 'User-Agent': 'python-requests/2.23.0', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*'}, 'origin': '122.44.107.152', 'id': 1}
<class 'dict'>
{'url': 'https://httpbin.org/stream/10', 'args': {}, 'headers': {'Host': 'httpbin.org', 'X-Amzn-Trace-Id': 'Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0', 'User-Agent': 'python-requests/2.23.0', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*'}, 'origin': '122.44.107.152', 'id': 2}
<class 'dict'>
{'url': 'https://httpbin.org/stream/10', 'args': {}, 'headers': {'Host': 'httpbin.org', 'X-Amzn-Trace-Id': 'Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0', 'User-Agent': 'python-requests/2.23.0', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*'}, 'origin': '122.44.107.152', 'id': 3}
<class 'dict'>
{'url': 'https://httpbin.org/stream/10', 'args': {}, 'headers': {'Host': 'httpbin.org', 'X-Amzn-Trace-Id': 'Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0', 'User-Agent': 'python-requests/2.23.0', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*'}, 'origin': '122.44.107.152', 'id': 4}
<class 'dict'>
{'url': 'https://httpbin.org/stream/10', 'args': {}, 'headers': {'Host': 'httpbin.org', 'X-Amzn-Trace-Id': 'Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0', 'User-Agent': 'python-requests/2.23.0', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*'}, 'origin': '122.44.107.152', 'id': 5}
<class 'dict'>
{'url': 'https://httpbin.org/stream/10', 'args': {}, 'headers': {'Host': 'httpbin.org', 'X-Amzn-Trace-Id': 'Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0', 'User-Agent': 'python-requests/2.23.0', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*'}, 'origin': '122.44.107.152', 'id': 6}
<class 'dict'>
{'url': 'https://httpbin.org/stream/10', 'args': {}, 'headers': {'Host': 'httpbin.org', 'X-Amzn-Trace-Id': 'Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0', 'User-Agent': 'python-requests/2.23.0', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*'}, 'origin': '122.44.107.152', 'id': 7}
<class 'dict'>
{'url': 'https://httpbin.org/stream/10', 'args': {}, 'headers': {'Host': 'httpbin.org', 'X-Amzn-Trace-Id': 'Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0', 'User-Agent': 'python-requests/2.23.0', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*'}, 'origin': '122.44.107.152', 'id': 8}
<class 'dict'>
{'url': 'https://httpbin.org/stream/10', 'args': {}, 'headers': {'Host': 'httpbin.org', 'X-Amzn-Trace-Id': 'Root=1-5e833f4e-52359d96cb0bc89a2a5c17d0', 'User-Agent': 'python-requests/2.23.0', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*'}, 'origin': '122.44.107.152', 'id': 9}
~~~

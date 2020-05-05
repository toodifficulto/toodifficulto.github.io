---
layout: post
title: 파이썬에서 비동기 프로그래밍 활용하기
tags: [python]
---

비동기를 사용하면 네트워크 I/O의 지연 때문에 낭비되는 시간을 줄일 수 있다. 예를 들어, 온라인 사전사이트에서 단어들의 의미를 크롤링하는 코드를 작성한다고 하자. 동기적인 방식으론 다음과 같이 작성한다.

# 동기적 방식

~~~python
import requests
import time
from bs4 import BeautifulSoup

def get_text_from_url(url):
    print(f"Send request to ... {url}")
    res = requests.get(url, headers = {'user-agent' : 'Mozilla/5.0'})
    print(f"Get response from ... {url}")
    text = BeautifulSoup(res.text, 'html.parser').text

    return text

if __name__ == '__main__':
    start = time.time()

    base_url = 'https://www.macmillandictionary.com/us/dictionary/american/{keyword}'

    keywords = ['hi', 'apple', 'banana', 'call', 'feel','hello', 'bye', 'like', 'love', 'environmental','buzz', 'ambition', 'determine']

    urls = [base_url.format(keyword = keyword) for keyword in keywords]

    for url in urls:
        text = get_text_from_url(url)
        print(text[:100].strip())

    end = time.time()

    print(f"time taken : {end - start}")
~~~

~~~
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/call
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/call
CALL (verb) American English definition and synonyms | Macmillan Dictionary
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/feel
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/feel
FEEL (verb) American English definition and synonyms | Macmillan Dictionary
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/hello
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/hello
HELLO (interjection) American English definition and synonyms | Macmillan Dictionary
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/bye
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/bye
BYE (interjection) American English definition and synonyms | Macmillan Dictionary
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/like
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/like
LIKE (adjective, adverb, conjunction, preposition) American English definition and synonyms
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/love
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/love
LOVE (verb) American English definition and synonyms | Macmillan Dictionary
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/environmental
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/environmental
ENVIRONMENTAL (adjective) American English definition and synonyms | Macmillan Dictionary
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/buzz
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/buzz
BUZZ (verb) American English definition and synonyms | Macmillan Dictionary
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/ambition
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/ambition
AMBITION (noun) American English definition and synonyms | Macmillan Dictionary
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/determine
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/determine
DETERMINE (verb) American English definition and synonyms | Macmillan Dictionary
time taken : 11.193539381027222
~~~
약 11초가 걸렸다. 왜냐하면 요청에 대한 응답을 받아야만 다음 요청을 할 수 있기 때문에 지연되는 시간이 발생한다.

&nbsp;
&nbsp;
&nbsp;

# 비동기적 방식 (Asyncio + requests)

이번에는 비동기적으로 가져와보자. requests는 비동기적으로 작성되지 않았기 때문에 loop.run_in_executor 를 통해 쓰레드를 만드는 방식으로 사용해야 한다.

~~~python
import requests
import time
import asyncio
from functools import partial
from bs4 import BeautifulSoup

async def get_text_from_url(url): # 코루틴 정의
    print(f"Send request to ... {url}")
    loop = asyncio.get_event_loop()

    # loop.run_in_executor는 kwargs를 사용할 수 없기 때문에
    # functools.partial를 활용
    request = partial(requests.get, url, headers = {'user-agent': 'Mozilla/5.0'})

    # ascyncio의 디폴트 쓰레드풀을 사용할 경우 첫번 째 인자로 None
    # 직접 쓰레드풀을 만들 경우 concurrent.futres.threadpool executor 사용
    res = await loop.run_in_executor(None, request)
    print(f'Get response from ... {url}')
    text = BeautifulSoup(res.text, 'html.parser').text
    print(text[:100].strip())

async def main():
    base_url = 'https://www.macmillandictionary.com/us/dictionary/american/{keyword}'
    keywords = ['hi', 'apple', 'banana', 'call', 'feel',
                'hello', 'bye', 'like', 'love', 'environmental',
                'buzz', 'ambition', 'determine']

    # 아직 실행된 것이 아니라 실행할 것을 계획하는 단계
    futures = [asyncio.ensure_future(get_text_from_url(base_url.format(keyword = keyword))) for keyword in keywords]

    await asyncio.gather(*futures)

if __name__ == "__main__":

    start = time.time()
    loop = asyncio.get_event_loop()
    loop.run_until_complete(main())
    end = time.time()

    print(f'time taken : {end - start}')    
~~~

~~~
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/call
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/feel
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/hello
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/bye
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/like
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/love
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/environmental
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/buzz
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/ambition
Send request to ... https://www.macmillandictionary.com/us/dictionary/american/determine
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/apple
APPLE (noun) American English definition and synonyms | Macmillan Dictionary
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/hello
HELLO (interjection) American English definition and synonyms | Macmillan Dictionary
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/hi
HI (interjection) American English definition and synonyms | Macmillan Dictionary
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/bye
BYE (interjection) American English definition and synonyms | Macmillan Dictionary
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/banana
BANANA (noun) American English definition and synonyms | Macmillan Dictionary
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/feel
FEEL (verb) American English definition and synonyms | Macmillan Dictionary
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/like
LIKE (adjective, adverb, conjunction, preposition) American English definition and synonyms
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/call
CALL (verb) American English definition and synonyms | Macmillan Dictionary
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/environmental
ENVIRONMENTAL (adjective) American English definition and synonyms | Macmillan Dictionary
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/determine
DETERMINE (verb) American English definition and synonyms | Macmillan Dictionary
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/love
LOVE (verb) American English definition and synonyms | Macmillan Dictionary
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/ambition
AMBITION (noun) American English definition and synonyms | Macmillan Dictionary
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/buzz
BUZZ (verb) American English definition and synonyms | Macmillan Dictionary
time taken : 1.6943330764770508
~~~
이번에는 약 1.43초가 걸렸다. 비동기적으로 작성한 코드가 동기적으로 작성한 코드보다 약 8배 이상의 성능개선이 있었다. 결과적으로 보면 **요청에 대한 응답을 기다리지 않고 바로 다른 요청을 하는 것** 을 확인할 수 있다.

&nbsp;
&nbsp;
&nbsp;

# 비동기적 방식(asyncio + aiohttp)
하지만 requests 모듈은 코루틴으로 만들어진 모듈이 아니기 때문에 위의 코드는 내부적으로 쓰레드를 만들어 동작한다. 따라서 요청의 수가 많아질 수록 컨텍스트 스위칭 비용이 발생한다.

비동기 HTTP 통신 라이브러리인 **aiohttp** 를 이용하면 코루틴을 이용한 비동기 방식을 이용할 수 있다.

~~~python
import time
import asyncio
import aiohttp # 먼저 설치 필요
from bs4 import BeautifulSoup

async def get_text_from_url(url): #코루틴 정의
    print(f"Send request to ... {url}")

    async with aiohttp.ClientSession() as sess:
        async with sess.get(url, headers={'user-agent': 'Mozilla/5.0'}) as res:
            text = await res.text()

    print(f'Get response from ... {url}')
    text = BeautifulSoup(text, 'html.parser').text
    print(text[:100].strip())

async def main():
    base_url = 'https://www.macmillandictionary.com/us/dictionary/american/{keyword}'
    keywords = ['hi', 'apple', 'banana', 'call', 'feel','hello', 'bye', 'like', 'love', 'environmental','buzz', 'ambition', 'determine']

    futures = [asyncio.ensure_future(get_text_from_url(base_url.format(keyword = keyword))) for keyword in keywords]

    await asyncio.gather(*futures)

if __name__ == "__main__":

    start = time.time()
    loop = asyncio.get_event_loop()
    loop.run_until_complete(main())
    end = time.time()

    print(f'time taken : {end-start}')
~~~

~~~
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/like
LIKE (adjective, adverb, conjunction, preposition)
American English definition and synonyms
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/call
CALL (verb) American English definition and synonyms | Macmillan Dictionary
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/love
LOVE (verb) American English definition and synonyms | Macmillan Dictionary
Get response from ... https://www.macmillandictionary.com/us/dictionary/american/buzz
BUZZ (verb) American English definition and synonyms | Macmillan Dictionary
time taken : 2.2038064002990723
~~~

위와 같이 하나의 웹사이트에서 크롤링할 때 비동기적인 방식을 사용하면 짧은 시간에 너무나도 많은 요청으로 인해 웹사이트에 나쁜 영향을 끼칠 수 있다. 아이피를 차단당할 수도 있으니 주의해야 한다. 만약 여러 웹사이트에서 크롤링한다면 비동기적인 방식을 고려해볼 수 있다.

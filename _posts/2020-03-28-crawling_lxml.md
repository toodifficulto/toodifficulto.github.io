---
layout: post
title: lxml사용법
tags:  [crawling]
---

# lxml이란
lxml은 XML과 HTML을 **parsing** 하는데 쉽고 강력한 API를 제공한다.

**사용법은 다음과 같다.**
~~~python
# section02-3
# 파이썬 크롤링 기초
# lxml 사용 기초 스크래핑(1)

# pip install lxml, requests

import requests
import lxml.html

def main():
    """
    네이버 메인 뉴스 스탠드 스크랩핑 메인함수
    """

    # 스크랩핑 대상 URL
    response = requests.get("https://www.naver.com")

    # GET 방식과 POST 방식
    # Query가 숨겨져서 보낼땐 POST방식
    # Query가 보여지는 경우 GET방식

    # 신문사 링크 리스트 획득
    urls = scrape_news_list_page(response)

    # 결과 출력
    for url in urls:
        # url 출력
        print(url)

        # 파일 쓰기
        # 생략

def scrape_news_list_page(response):

    # URL 리스트 선언
    urls = []

    # 태그 정보 문자열 저장
    # CSS 선택자 사용!
    root = lxml.html.fromstring(response.content)

    for a in root.cssselect('.api_list .api_item a.api_link'):
        # 링크
        url = a.get('href')
        urls.append(url)

    return urls

# 스크랩핑 시작
if __name__ == "__main__":
    main()
~~~

# session과 lxml을 함께 활용

~~~python
# section 02-3
# 네이버 뉴스 스탠드 스크래핑(2)
# 네이버 메인 뉴스 정보 스크래핑
# 신문사 정보 딕셔너리 출력
# session 사용
# xpath 활용

# session을 이용해서 이 사람이 로그인되어 있는 지 체크한다.
# 한 번 로그인을 하면 브라우저를 꺼도 다시 로그인이 되어 있다.
#
import requests
from lxml.html import fromstring, tostring

def main():
    """
    네이버 메인 뉴스 스탠드 스크랩핑 메인함수
    """

    # 세션 사용
    session = requests.Session()

    # 스크랩핑 대상 URL
    response = session.get("https://www.naver.com")

    # GET 방식과 POST 방식
    # Query가 숨겨져서 보낼땐 POST방식
    # Query가 보여지는 경우 GET방식

    # 신문사 링크 딕셔너리 획득
    urls = scrape_news_list_page(response)

    # 딕셔너리 확인
    # print(urls)

    # 결과 출력
    for name, url in urls.items():

        # url 출력
        print(name, url)

        # 파일 쓰기
        # 생략


def extract_contents(dom):

    # 링크 주소
    link = dom.get("href")

    # 신문사 명
    name = dom.xpath('./img')[0].get('alt') #

    return name, link

def scrape_news_list_page(response):

    # URL 딕셔너리 선언
    urls = {}

    # 태그 정보 문자열 저장
    # CSS 선택자 사용!
    root = fromstring(response.content)

    for a in root.xpath('//ul[@class="api_list"]/li[@class="api_item"]/a[@class="api_link"]'):
        # a 구조 확인
        # a의 문자열 출력
        # print(tostring(a, pretty_print=True))

        name, url = extract_contents(a)

        # 딕셔너리 삽입
        urls[name] = url

    return urls

# 스크랩핑 시작
if __name__ == "__main__":
    main()
~~~

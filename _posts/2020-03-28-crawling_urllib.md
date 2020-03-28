---
layout: post
title: urllib 사용법 및 기본 스크래핑
tags:  [crawling]
---
# urllib이란?
urllib은 URL과 관련된 모듈들을 모아놓은 package이다.

### 1. urllib.reuest : url을 열고 읽는 데 사용.

### 2. urllib.error : urllib.request에서 일어난 error handling을 처리하는 데 사용.

### 3. urllib.parse : url을 parse하는데 사용한다.

### 4.urlib.robotparser : robots.txt를 parse하는데 사용한다.


**requests는 cookie값을 통해서 session을 유지하는 대신 통신을 한 번만 주고 받는다.**

**parsing** 이란 가지고 온 데이터에서 내가 원하는 데이터만 추출하는 것이다.

&nbsp;
&nbsp;
&nbsp;
&nbsp;


# req.urlretrieve 함수 기초 사용법
urllib.request.urlretrieve() 함수는 실제 데이터와 header 두 개를 return한다.

~~~python

import urllib.request as req

# 파일 URL
img_url = 'https://search.pstatic.net/common/?src=http%3A%2F%2Fblogfiles.naver.net%2F20141230_163%2Fcnldjqxhrxhr_1419924109977eNUlz_PNG%2F%25BD%25BA%25C5%25A9%25B8%25B0%25BC%25A6_2013-06-08_%25BF%25C0%25C8%25C4_5.25.07.png&type=b400'
html_url = 'http://google.com'

# 다운받을 경로
save_path1 = 'test1.jpg'
save_path2 = 'index.html'

# 예외 처리
try:
    # req.urlretrieve()는 두 개를 return한다.
    # cookie값을 통해서 session을 유지해야 한다. <== requests는 한 번만 주고 받는다.
    # 먼저 website에서 모든 정보를 가지고 오는 게 crawling이다.
    # 그리고 가지고 온 정보에서 내가 원하는 데이터만 빼내는 것이 parsing이다.

    file1, header1 = req.urlretrieve(img_url, save_path1)
    file2, header2 = req.urlretrieve(html_url, save_path2)
except Exception as e:
    print('Download failed')
    print(e)
else:
    # Header 정보 출력
    print(header1)
    print(header2)

    # 다운로드 파일 정보
    print("Filename1 {}".format(file1))
    print("Filename2 {}".format(file2))
    print()

    # 성공
    print('Downloaded success!')
~~~

&nbsp;
&nbsp;
&nbsp;
&nbsp;

# urlopen 함수 기초 사용법
urllib.request에 포함되어 있으며 python3에서는 reqeust까지 모두 import해줘야 에러가 안 난다.

해당 URL을 열고 데이터를 얻을 수 있는 함수와 클래스를 제공하며 HTTP를 통해 웹 서버에 데이터를 얻는 데 많이 사용된다.

urlopen() 에서는 다음과 같이 몇가지 **메서드** 를 지원한다.

**1.urlopen().read([nbytes]) :** nbyte의 데이터를 바이트 문자열로 읽음 ​

**2.urlopen().readline() :** 한 줄의 텍스트를 바이트 문자열로 읽음

**3. urlopen().​info() :** URL에 연관된 메타 정보를 담은 매핑 객체를 반환​

**4.urlopen().getcode() :** HTTP 응답 코드를 정수로 반환( 200, 404 )

**5. urlopen().close() :** 연결을 닫는다

~~~python
# section02 - 2
# 파이썬 크롤링 기초
# urlopen 함수 기초 사용법

import urllib.request as req
from urllib.error import URLError, HTTPError

# 다운로드 경로 및 파일명
path_list = ['test1.jpg', 'index.html']

# 다운로드 리소스 url
target_url = ['http://blogfiles.naver.net/MjAxOTEyMDZfNTgg/MDAxNTc1NjMzNTYyNTIz.HvBOiFNA2r954L1g1CQW79cDihiJSePgOtU2nR8Gml8g.InJjog7XBckUTu2Gs5246ZkcAmmUTpeW4w3AZuxhpJUg.JPEG.lee130101/%BE%C6%C0%CC%C0%AF%C5%A9%B7%D3%B8%DE%C0%CE%C4%C6_RGB-2%28%B7%CE%B0%ED%29.jpg', \
    'http://google.com']

for i, url in enumerate(target_url):
    # 예외 처리
    try:

        # 웹 수신 정보 읽기
        response = req.urlopen(url)

        # 수신 내용
        contents = response.read()

        print("-----------------------------------------------")

        # 상태 정보 중간 출력
        print('Header Info-{} : {}'.format(i, response.info()))
        print('HTTP Status Code : {}'.format(response.getcode()))
        print()
        print("-----------------------------------------------")


        with open(path_list[i], 'wb') as c:
            c.write(contents)


    except HTTPError as e:
        print("Download failed")
        print("HTTPError Code : ", e.code)

    except URLError as e:
        print("Download failed")
        print("URL Error Reason : ", e.reason)

    else:
        print()
        print("Download Succeeds.")
~~~

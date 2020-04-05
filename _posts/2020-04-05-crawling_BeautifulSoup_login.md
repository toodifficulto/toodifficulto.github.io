---
layout: post
title: BeautifulSoup실습 - login하기
tags: [crawling]
---
이제 Requests와 BeautifulSoup를 사용해서 **다나와** 에 로그인을 해보자.

로그인 페이지에서 아이디와 패스워드를 입력해보자.
![Alt text](/public/post/2020_04_05_BeautifulSoup_Login/danawa_login.PNG)

그러면 Network탭에 login Requests를 클릭해보자.
![Alt text](/public/post/2020_04_05_BeautifulSoup_Login/Login_Post.PNG)

Login Requests은 POST방식이다.
![Alt text](/public/post/2020_04_05_BeautifulSoup_Login/post.PNG)

Form-Data를 한번 살펴보자. 다음과 같다. redirectUrl은 이 페이지로 오기 전에 있던 사이트이다. 그리고 id와 password값도 들어 가 있다.
![Alt text](/public/post/2020_04_05_BeautifulSoup_Login/form_data.PNG)

이제 이런 정보를 바탕으로 requests로 login을 해보자.

&nbsp;
&nbsp;
&nbsp;

# 실습
아래 requests를 보낼 때 login_info 값과 headers값을 함께 주어야 한다. 참고로, id값과 password는 본인의 다나와 사이트 아이디와 password값을 사용하자.
~~~python
import requests as req
from fake_useragent import UserAgent
from bs4 import BeautifulSoup


# Login 정보 (개발자 도구)
login_info = {
    "redirectUrl": "http://www.danawa.com/?src=adwords&kw=GA0000020&gclid=CjwKCAjwvZv0BRA8EiwAD9T2VXeCafelXx2YFiBJXID4fe4jr6cw8VSTQJdjHS2lqW71Bv00Hybw9xoCDXsQAvD_BwE",
    "loginMemberType" : "general",
    "id" : "abcd",
    "password" : "1234"
}

# Headers 정보
headers = {
    "User-Agent" : UserAgent().chrome,
    "Referer" : "https://auth.danawa.com/login?url=http%3A%2F%2Fwww.danawa.com%2Fmember%2FmyPage.php"
}

with req.Session() as s:
    # Request(로그인 시도)
    res = s.post('https://auth.danawa.com/login', login_info, headers=headers)

    # 로그인 시도 실패 시 예외
    if res.status_code != 200:
        raise Exception("Login Failed")

    # 본문 수신 데이터 확인
    print(res.content.decode("UTF-8"))

    # bs4 초기화
    soup =  BeautifulSoup(res.text, 'html.parser')

    check_name = soup.find('p', class_ = 'user')

    if check_name is None:
        raise Exception('Login failed. Wrong Password.')


    # 선택자 사용
    info_list = soup.select("div.my_info > div.sub_info > ul.info_list > li")

    # 확인
    print(info_list)

    # 이부분에서 재요청, 파일다운로드, DB저장, 파일 쓰기(엑셀)

    # 제목
    print()
    print("***** My Info *****")

    for v in info_list:
        # 속성 메소드 확인
        # print(dir(v))

        # 필요한 텍스트 추출
        proc, val = v.find('span').string.strip(), v.find('strong').string.strip()
        print("{} : {}".format(proc, val))


~~~

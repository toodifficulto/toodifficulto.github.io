---
layout: post
title: Django 설정하기
tags:  [python-web-development]
---
# Visual Studio Code 다운로드

먼저 이 프로젝트는 Visual Studio Code를 IDE로 사용한다. 그래서 먼저 아래의 사이트에서 Visual Studio Code를 다운로드 받자.

[Visual Studio Code 다운로드](https://code.visualstudio.com/download)

&nbsp;
&nbsp;
&nbsp;

# Python 다운로드
그런다음에 Python을 설치하자.

[Python 다운로드](https://www.python.org/downloads/)

그리고 cmd에서 python을 사용할 수 있게 환경변수 설정도 하자.
환경변수 설정에 관해서는 아래 페이지 참고

[파이썬 환경변수](https://wxmin.tistory.com/121)

&nbsp;
&nbsp;
&nbsp;

# Visual Studio Code에서 Python 사용하기
python을 설치하고 나서 Visual Studio Code를 켜보자. 그리고 옆에 Extension이 있는데 여기서 python을 검색해서 가장 상위에 있는 python extension을 선택해서 download하자.

![Alt text](/public/post/2020_02_06_python_web_django/pic_2.PNG)


&nbsp;
&nbsp;
&nbsp;

# PIP3 설치하기
python의 패키지들을 쉽게 다운로드 할 수 있게 해주는 것이 pip3인데 이걸 설치해야 한다. conda에서는 pip3대신 conda를 사용하지만 이번에는 pi3를 직접 다운로드해서 설치해보자.

아래 블로그를 참조하면 pip3를 설치할 수 있다. 그리고 **pip3를 설치한 다음 환경변수를 설정해주어야 한다.**

[pip3 설치하기](https://m.blog.naver.com/varkiry05/221207337881)

그러면 아래 Visual studio Code terminal에서 pip3를 했을 때 잘 되는 것을 볼 수 있다.

![Alt text](/public/post/2020_02_06_python_web_django/pic_1.PNG)


&nbsp;
&nbsp;
&nbsp;

# 가상환경 설정해주기
새로운 프로젝트를 할 때마다 여러 라이브러리를 설치해야 하는데 이럴 때 마다 가상환경안에서 사용하면 dependecy문제도 줄어들고 여러모로 편하다. 그래서 virtualenv를 사용해서 가상환경을 만들고 거기서 django를 시작해보자.

먼저 virtualenv를 pip3로 설치한다.
~~~python
pip3 install virtualenv
~~~

그런다음 fcdjango_venv라는 가상환경을 만든다. 그러면 옆에 fcdjango_venv라는 새로운 파일이 만들어 진것을 볼 수 있다.

![Alt text](/public/post/2020_02_06_python_web_django/pic_3.PNG)

그런 다음 fdcjango_venv를 activate해주어야 한다. 그러나 내 컴퓨터에서는 아래와 같은 에러가 떴다.

~~~
.\fcdjango_venv\Scripts\activate.ps1 : 이 시스템에서 스크립트를 실행할 수 없으므로 C:\Users\ASUS\Programming\Python\web_
programming\fcdjango_venv\Scripts\activate.ps1 파일을 로드할 수 없습니다. 자세한 내용은 about_Execution_Policies(https:/
/go.microsoft.com/fwlink/?LinkID=135170)를 참조하십시오.
~~~

그래서 찾아본 결과 powershell에서 executionPolicy를 unrestricted로 바꾸어야 한다는 것을 알았다. 그래서 powershell에서 아래와 같이 바꾸어주었다.

![Alt text](/public/post/2020_02_06_python_web_django/pic_4.PNG)

[참고사이트](https://gosmcom.tistory.com/115)

그리고 나서 다시 해보니 잘 되었다.

![Alt text](/public/post/2020_02_06_python_web_django/pic_5.PNG)

그리고 나서 startproject를 해주고

~~~
django-admin startproject fc_community
~~~

startboard도 만들어 주었다.
~~~
django-admin startapp_board
~~~

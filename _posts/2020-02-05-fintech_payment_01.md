---
layout: post
title: 간편결제 프로그래밍 실습 (1) 개발환경 설정
tags:  [fintech]
---
**핀테크 서비스 개발 실습 초급 과정에서 배운 내용을 정리**

이 과정에서 사용되는 개발 환경과 구조이다. DBMS는 MYSQL을 사용하고 Server side에는 Node JS를 기반으로 Server framework는 ExpressJS를 사용한다. Front-end에는 Bootstrap과 Jquery를 사용할 예정이다.

![Alt text](/public/post/2020_02_05_fintech_1/pic1.PNG)

# MYSQL 설치하기.

아래 사이트에서 Community edition 버전 MYSQL을 설치한다.

[https://www.mysql.com/downloads/](https://www.mysql.com/downloads/)

**MYSQL 설치시 주의사항**
ROOT 계정 비밀번호는 반드시 기억해야 한다.

그리고 이후 실습과정에서 Server와 연동 편의성을 위해 Legacy 방식의 데이터베이스 인증을 사용한다.

&nbsp;
&nbsp;
&nbsp;

# NodeJS 설치하기
Node.js는 Chrome V8 Javascript 엔진으로 빌드된 Javascript 런타임이다.

즉, **자바스크립트로 서버를 개발할 수 있도록 해준다.**

NodeJS에 대한 자세한 설명은 아래 사이트 참조.

[Node.js란 무엇인가?](https://asfirstalways.tistory.com/43)

Node.JS는 아래 사이트에서 설치하면 된다.

[Node.JS 설치 사이트](https://nodejs.org/ko/download/)

설치하고 나서, node를 쳐본다. 그러면 아래와 같이 뜨면 성공적으로 설치가 된 것이다.

![Alt text](/public/post/2020_02_05_fintech_1/pic2.PNG)


&nbsp;
&nbsp;
&nbsp;

# Visual Studio Code 설치
Microsoft에서 만든 텍스트 편집기이다.. VSCode라고 줄여서 말하기도 한다. "현대적인 웹 및 클라우드 응용 프로그램을 빌드 및 디버깅하기 위해 재정의되고 최적화된 코드 편집이다.

아래 사이트에서 다운로드

[VS 다운로드 사이트](https://code.visualstudio.com/)

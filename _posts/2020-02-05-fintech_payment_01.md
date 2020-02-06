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

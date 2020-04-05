---
layout: post
title: Rest API란?
tags: [crawling]
---
# API(Application Programming Interface)란?
응용프로그램에서 사용할 수 있도록, 운영체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 **인터페이스** 를 뜻한다.

예를 들어, 회사에서 파일제어 응용 프로그램을 만들었다고 하자. **회사 외부에서 해당 프로그램을 사용하고 싶을 때 외부 사용자가 해당 소스 및 데이터베이스에 접근하면 문제가 생긴다.** 이런 문제를 해결하기 위해 **API를 사용하는 것이다.**

**즉, API를 통해 소스 및 데이터베이스는 접근하지 못하게 하면서 해당 프로그램을 사용할 수 있도록 기능을 제공하는 것이다!**

[출저 사이트](https://hyunalee.tistory.com/1)

&nbsp;
&nbsp;
&nbsp;

# REST API(Representational State Tranfser)란?
과거에는 브라우저가 웹서버에 웹페이지를 요청하여 클라이언트를 제공하며 웹페이지가 작동하였다.

**하지만 최근에는 SPA(Single-Page-Application)를 이용하여 클라이언트(대표적으로 React, Vue, Angular)를 구현하며, 클라이언트를 서버와 분리하여 클라이언트 로직을 분명히 한다. 또한 서버에 API를 요청함으로써 웹 어플리케이션을 개발한다.**

Rest API에 REST는 **Representational State Transfer의** 약자로 소프트웨어 프로그램 아키텍쳐의 한 형식이다.

REST api의 등장은 2000년도에 HTTP의 주요 저자 중 한 사람인 로이 필딩이 그 당시 웹(HTTP) 설계의 우수성에 비해 제대로 사용되어지지 못하는 모습에 안타까워하며 웹의 장점을 최대한 활용할 수 있는 아키텍처로써 REST를 발표 하였다.

&nbsp;
&nbsp;
&nbsp;

# Rest 구성
Rest API는 다음의 구성으로 이루어져 있다.

* 자원(Resource) : URL
* 행위 (Verb) : HTTP Method
* 표현 (Representations)


&nbsp;
&nbsp;
&nbsp;

# REST 특징
REST 아키택처를 이용하여 API를 설계하면 다음과 같은 이점이 있다.

#### 1. 클라이언트/서버 구조
클라이언트는 유저와 관련된 처리를, 서버는 REST api를 제공함으로써 각각의 역할이 확실하게 구분되고 일관적인 인터페이스로 분리되어 작동할 수 있게 한다.

#### 2. 무상태성(Stateless)
REST는 HTTP의 특성을 이용하기 때문에 무상태성을 갖는다. 즉 서버에서 어떤 작업을 하기 위해 상태정보를 기억할 필요가 없고 들어온 요청에 대해 처리만 해주면 되기 때문에 구현이 쉽고 단순해진다.


#### 3. 캐시 처리 기능(Cacheable)
HTTP라는 기존 웹표준을 사용하는 REST의 특징 덕분에 기본 웹에서 사용하는 인프라를 그대로 사용 가능하다.

#### 4. 자체 표현 구조 (Self-descriptiveness)
JSON을 이용한 메세지 포맷을 이용하여 직관적으로 이해할 수 있고 REST api메세지만으로 그 요청이 어떤 행위를 하는 지 알 수 있다.

#### 5. 계층화 (Layered System)
클라이언트와 서버가 분리되어 있기 때문에 중간에 프록시 서버, 암호화 계층등 중간매체를 사용할 수 있어 자유도가 높다.

#### 6. 유니폼 인터페이스(Uniform)
Uniform Interface는 Http 표준에만 따른다면 모든 플랫폼에서 사용이 가능하며, URL로 지정한 리소스에 대한 조작을 가능하게 하는 아키텍쳐 스타일을 말한다.

&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 자원에 대한 행위는 HTTP Method로 표현한다.
아래가 대표적으로 사용하는 4가지 Method이다.

| HTTP Method | 역할 |
|---|:---:|
| `GET` | Get을 통해 리소스를 조회한다.|
| `POST` | POST를 통해 해당 URL를 요청하면 리소스를 생성한다. |
| `PUT` |PUT을 통해 해당 리소스를 수정한다.|
| `DELETE` |DELETE를 통해 해당 리소스를 삭제한다.|

[출저 사이트](https://velog.io/@wlsdud2194/HTTP-REST-API-%EB%9E%80)

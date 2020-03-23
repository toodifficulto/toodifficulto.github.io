---
layout: post
title: AWS와 Docker의 중요성
tags:  [aws-docker]
---
# AWS란?
![Alt text](/public/post/2020_03_23_aws_docker_intro/aws.jpg)
AWS는 __Amazon Web Service__ 의 약자이다.
전통적으론 많은 기업들이 IDC(Internet Data Center)라고 해서 직접 서버를 구매하고 운영하는 방식으로 서버를 관리해왔었다. 하지만 점차 클라우드 서버의 데이터 센터가 커지고 속도가 향상되자 자체 서버 대신 AWS를 사용하게 되었다.

&nbsp;
&nbsp;
&nbsp;
&nbsp;

# Docker란?
![Alt text](/public/post/2020_03_23_aws_docker_intro/docker.png)
도커는 2013년 docker.inc 에서 출시한 오픈소스 컨테이터 프로젝트이다. 도커를 사용하는 이유는 __도커 없이도 서비스를 올릴 수 있지만 도커를 이용해 패키징을 하게 되면 소프트웨어를 제어하는 데 있어서 훨씬 용이하기 때문이다.__ 즉, __메모리나 IO을 이용하는 방식을 제어하거나 어플리케이션을 네트워크에 노출하는 방식 그리고 접근 권한을 통제할 수 있게 된다.__ 이런 다양한 측면에서 서비스를 개발/운영함으로써 도커를 통해 이미지를 만들고 만들어진 이미지를 컨테이너에 올려서 서비스를 올릴 수 있다.

&nbsp;
&nbsp;
&nbsp;
&nbsp;

# AWS의 장점과 단점
![Alt text](/public/post/2020_03_23_aws_docker_intro/aws_pro_con.PNG)

#### 장점

1. 장점으론 탄력적인 운영이 가능하다. 즉, __작은 규모로 부터 시작할 수 있고 규모를 키워나갈 수 있다.__

2. __다양한 API 커맨드를 제공한다.__ 필요한 부분이 있으면 그때 그때 따라 추가하거나 붙이고 없애고를 할 수 있다. 즉, AWS 내에 있는 수많은 기능을 돈만 내면 사용이 가능하다.

3. __유연한 클라우드 호스팅 서비스를 제공한다.__ 규모도 규모이지만 서버의 개수를 늘리거나 줄이거나 하는 부분을 빠르게 할 수 있다.

4. Storage, RDS(아마존 관계형 데이터베이스 서비스), VCP등과 통합 가능하다.

5. 서비스를 안정적으로 제공가능하다. 아마존에서 보안을 인증해준다.

#### 단점
1. __베어메탈__ 의 성능을 원하는 경우 그만큼의 IO에 대한 코드들이 필요하다.

2. __서비스가 늘 확정성을 띌 필요가 없다.__ 작은 서비스나 확장성이 크지 않은 경우 AWS 보다 기존의 환경들이 더 나을 때가 있다.


__베어메탈이란?__

IT 업계에서 사용하는 베어메탈의 의미는 “어떤 소프트웨어도 담겨 있지 않은 하드웨어”를 의미하며, 다른 관점에서는 “하드웨어만을 구매할 수 있는 제품”을 의미한다.

&nbsp;
&nbsp;
&nbsp;
&nbsp;

# AWS의 종류
![Alt text](/public/post/2020_03_23_aws_docker_intro/aws_type.PNG)

![Alt text](/public/post/2020_03_23_aws_docker_intro/aws_type_2.PNG)

#### 1. EC2(Elastic Compute Cloud)
__Amazon Elastic Compute Cloud(EC2)는__ 안전하고 크기 조정이 가능한 컴퓨팅 파워를 클라우드에서 제공하는 웹 서비스이다. 개발자가 더 쉽게 웹 규모의 클라우드 컴퓨팅 작업을 할 수 있도록 설계되었다.

https://postitforhooney.tistory.com/entry/%EC%9E%90%EC%A3%BC-%EC%82%AC%EC%9A%A9%EB%90%98%EB%8A%94-AWS-%EA%B8%B0%EB%8A%A5-%EB%B0%8F-%EC%9E%A5%EC%A0%90-%EC%A0%95%EB%A6%AC

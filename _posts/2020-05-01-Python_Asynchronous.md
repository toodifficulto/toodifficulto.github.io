---
layout: post
title: 파이썬과 비동기 프로그래밍
tags: [python]
---

# 비동기(Asynchronous)란?
예를 들어 10대의 세탁기와 10대의 커피포트에 물을 끓인다고 하자.

**첫번째 방법**
* 1번 세탁기를 돌린다.
* 1번 세탁기가 완료될 때까지 기다린다.
* 1번 세탁기에서 빨래를 수거한다.
* 2번 세탁기를 돌린다.
* 2번 세탁기가 완료될 때까지 기다린다.
... (위의 과정 반복)
* 2번 세탁기에서 빨래를 수거한다.
* 10번 세탁기를 돌린다.
* 10번 세탁기가 완료될 때까지 기다린다.
* 10번 세탁기에서 빨래를 수거한다.
* 1번 커피포트에 물을 끓인다.
* 1번 커피포트의 물로 녹차를 탄다.
* 1번 커피포트의 물이 끓기를 기다린다.
* 2번 커피포트의 물을 끓인다.
* 2번 커피포트의 물이 끓기를 기다린다.
* 2번 커피포트의 물로 녹차를 탄다.
... (위의 과정 반복)
* 10번 커피포트의 물을 끓인다.
* 10번 커피포트의 물이 끓기를 기다린다.
* 10번 커피포트의 물로 녹차를 탄다.

**두번째 방법**
* 1번 세탁기를 돌린다.
* 1번 세탁기가 완료되길 기다리지 않고 2번 세탁기를 돌린다.
... (위의 과정 반복)
* 9번 세탁기가 완료되길 기다리지 않고 10번 세탁기를 돌린다.
* 10번 세탁기가 완료되길 기다리지 않고 1번 커피포트에 물을 끓인다.
* 1번 커피포트의 물이 끓길 기다리지 않고 2번 커피포트에 물을 끓인다.
... (위의 과정 반복)
* 10번 커피포트에 물을 끓인다.
* 각 작업이 완료되는 순서대로 처리한다. (빨래 수거/녹차 타기)

두번째 방식으로 일을 처리하는 것이 **비동기적 처리** 라 하고, 첫 번째 방식으로 일을 처리하는 것을 **동기적 처리** 라고 한다.

비동기적 처리 방식과 동기적 처리 방식의 가장 큰 차이는 **기다리는 시간의 존재 여부이다.** 비동기적 처리방식에는 기다리는 시간에 바로 다른 작업을 하는 반면 동기적 처리 방식에선 하나의 작업을 시작했으면 그 작업이 끝날 때 까지 다른 작업을 실행하지 않는다. 

프로그래밍에서도 응답을 기다려야 하는 일이 발생 했을 때 기다리지 않고 바로 다른 작업을 하는 것을 **비동기적 프로그래밍** , 응답이 올 때까지 기다린 이후에 다른 작업을 하는 것을 **동기적 프로그래밍** 이라고 한다.
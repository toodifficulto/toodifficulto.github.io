---
layout: post
title: Python Sequence형
tags: [Python]
---
# 시퀀스형
파이썬의 중요한 핵심 프레임워크 -> __시퀀스(Sequence),__ __반복(Iterator),__ __함수(Functions),__ __클래스(Class)__ 이 있다. 그 중 **시퀀스형** 에 대해 공부해보자.


시퀀스형의 자료형은 무언가의 조합으로 이루어진 자료형이라고 생각하면 된다. 3가지의 기본 시퀀스형(리스트, 튜플, 레인지) 가 대표적이다.

시퀀스형은 다음과 같이 __container 시퀀스형과__ __flat 시퀀스형으로__ 나뉘어진다.

* __컨테이너 시퀀스__ : 서로 다른 자료형의 항목들을 담을 수 있는 list, tuple, collections.deque 형태

* __균일 시퀀스__ : 하나의 자료형만 담을 수 있는 str, bytes, memoryview, array.array 형태

컨테이너 시퀀스는 객체에 대한 참조를 담고 있으며 객체는 어떠한 자료형도 될 수 있다. 하지만 균일 스퀀스는 객체에 대한 참조 대신 자신의 메모리 공간에 각 항목 값을 직접 담는다. 따라서, 균일 시퀀스가 메모리를 더 적게 사용하지만, 문자, 바이트, 숫자 등 기본적인 자료형만 저장할 수 있다.

시퀀스는 다음과 같이 가변성에 따라 분류할 수도 있다.

* __가변 시퀀스__ : list, bytearray, array.array, collections.deque, memoryview 등

* __불변 시퀀스__ : tuple, str, bytes 등

출처: https://excelsior-cjh.tistory.com/164

&nbsp;
&nbsp;




### 리스트(List)
- 리스트는 **가변** 시퀀스형이다.
- 일반적으로 유사한 항목의 모음을 저장하는데 사용
- 다양한 방법을 통해 생성가능
  * 대괄호를 통해 빈 리스트 생성 : []
  * 리스트 컴프리헨션 이용 : [x for x in range(0,20)]
  * 형 생성자 이용 : list(), list((1,2,3))

&nbsp;
&nbsp;

### 튜플(tuple)
- 튜플은 **불변** 시퀀스형이다.
- 보통 서로 다른 데이터를 저장할 때 사용한다. (예, enumerate()함수로 생성된 2-튜플)
- 같은 형태의 데이터의 불변 시퀀스가 필요한 경우 종종 사용한다.
- 튜플 생성은 다양한 방법을 통해 가능하다.
  * 소괄호를 통해 빈 리스트 생성 : ()
  * 단일 항목 튜플을 위해 마지막에 쉼표 붙이기 : a, 또는 (a,)
  * 항목을 쉼표로 구분 : a,b,c 또는 "(a,b,c)"
  * 형 생성자 이용 : tuple() 또는 tuple((1,2,3,...))

> 실제 튜플을 만드는 것은 괄호가 아니라, **쉼표** 이다! 괄호는 문법상 모호함을 피가히 위함이다!

&nbsp;
&nbsp;

### 레인지(range)
- 숫자의 불변 시퀀스형을 나타낸다.
- for 루프에서 특정 횟수만큼 반복하는데 자주 사용한다.
- range를 생성할 때의 형태이다.
  * range(stop) : 0부터 stop전까지 생성
  * range(start, stop) : 0부터 stop전 까지 1씩 증가시켜가며 생성.
  * range(star, stop, step) : start부터 stop 전 까지 step만큼 증가시켜가며 생성.(start의 기본 값은 0이며, step의 기본값은 1이다)
  * 표현범위크기와 무관하게 리스트나 튜플보다 메모리 사용이 작다.
  * == 나 =! 로 검사할 경우 시퀀스처럼 비교하게 된다. 즉, 범위가 나타내는 같이 같으면 같다고 취급한다.


&nbsp;
&nbsp;

### 공통 시퀀스 연산

![Alt text](/public/post/2020_04_12_Python_sequence/method.PNG)


[출저](https://kongdols-room.tistory.com/24)

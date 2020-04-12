---
layout: post
title: Python Namedtuple
tags: [python]
---

일반적으로 튜플은 불변형이며 한번 선언 후 데이터값을 변경하는 것은 불가능하다. 그래서 튜플을 사용하려면 **변하지 않는 값들을 모아놓고 사용하는 것이 편하다.** 튜플은 특성상 리스트보다 처리속도가 빠르다.

### 튜플 사용
튜플을 사용하면 일반적으로 주석으로 어떤 값이 무엇을 나타내는지 알려주는 것이 좋다.

예를 들어서, 점 두 개를 선언하고 두 점 사이의 거리를 구해보고 싶다고 하자.

~~~python
# 0은 x, 1은 y로 따로 주석을 달아놓아야 한다.
# 그래서 따로 주석이 필요 없이 각 값에 이름을 붙이고 싶다. -> namedtuple
pt1 = (1.0, 5.0)
pt2 = (2.4, 1.5)

from math import sqrt

line_leng1 = sqrt((pt2[0] - pt1[0]) ** 2  + (pt2[1] - pt1[1]) ** 2)

print('Ex1-1 : ', line_leng1)
print('\n')
~~~

~~~
Ex1-1 :  3.7696153649941526
~~~

그래서 따로 위처럼 주석으로 값이 어떤 것인지 알려주는 대신 각 값에 이름을 붙일 수 있게 해주는 것이 바로 **namedtuple** 이다!

&nbsp;
&nbsp;
&nbsp;

# Namedtuple
네임드 튜플은 정말 자주 사용되는 데이터 타입이다. 이유는 아래와 같은 특성을 가지고 있기 때문이다.

- 튜플의 기본 성질인 불변 객체
- 일반 Class(객체) 형태보다 적은 메모리 사용
- 다양한 접근법 지원(괄호,.)
- Dictionary Key와 같이 사용


**사용방법은 아래와 같다.**

네임드 튜플 선언은 클래스 형태로 받아들인다. 그래서 클래스 형태인것 같지만 메모리는 훨씬 적게 잡아먹는다. 또한 튜플의 **불변 성질** 도 가지고 있다.
~~~python
from collections import namedtuple

# 네임드 튜플 선언
# 네임드 튜플 선언은 클래스 형태로 받아들인다.
Point = namedtuple('Point', 'x y')

pt1 = Point(1.0, 5.0)
pt2 = Point(2.4, 1.5)

line_leng2 = sqrt((pt2.x - pt1.x) ** 2  + (pt2.y - pt1.y) ** 2)

print('Ex1-2 : ', line_leng2)
print('Ex1-3 : ', line_leng1 == line_leng2)
print('\n')
~~~

~~~
Ex1-2 :  3.7696153649941526
Ex1-3 :  True
~~~

&nbsp;
&nbsp;
&nbsp;

### 네임드 튜플 선언 방법

1. 리스트 형태로도 받는다.
~~~python
Point1 = namedtuple('Point', ['x', 'y']) # 리스트 형태로도 받는다.
~~~

2. 코마로 구분해서도 가능하다.

~~~python
Point2 = namedtuple('Point', 'x, y') # 코마로 구분해서도 가능하다.
~~~

3. 공백도 가능하다.
~~~python
Point3 = namedtuple('Point', 'x y') # 공백도 가능하다.
~~~

&nbsp;
&nbsp;
&nbsp;

### 객체 생성 방식
객체 생성은 다음과 같다.
~~~python
p1 = Point1(x=10, y=35)
p2 = Point2(20, 40)
p3 = Point3(45, y=20)
p4 = Point4(10, 20, 30, 40)
~~~
또 dictionary를 unpacking 할 수 도 있다.

~~~python
# 그래서 dictionary를 unpacking할때는 **를 붙여줘야 한다.
p5 = Point3(**temp_dict)
~~~

&nbsp;
&nbsp;
&nbsp;

### 사용방법
인덱스로도 접근이 가능하다. 왜냐하면 튜플의 성질을 가지고 있기 때문이다.

혹은 클래스 변수 접근 방식도 가능하다.

Unpacking도 가능하다.
~~~python
# 인덱스로도 접근이 가능하다. 튜플의 성질도 가지고 있기 때문이다!
print('Ex3-1 : ', p1[0] + p2[1])
print('Ex3-2 : ', p1.x + p2.y) # 클래스 변수 접근 방식
print('\n')

# Unpacking
x, y = p3
print('Ex3-3 : ', x+y)
~~~

&nbsp;
&nbsp;
&nbsp;

### 네임드 튜플 메소드

-  make() : 새로운 객체를 생성한다.

~~~python
temp = [52, 38] # 시퀀스형이면서 이터레이터의 특징을 가지는 리스트.

# _make() : 새로운 객체를 생성
p4 = Point1._make(temp)
~~~

- fields : 필드 네임 확인. namedtuple이 있을 무엇이 있는지 알아 볼 수 있다.

~~~python
# _fields : 필드 네임 확인,  namedtuple이 있을 때 무엇이 있는지 알아보는 방법
print('Ex4-2 : ', p1._fields, p2._fields, p3._fields)
~~~

- asdict() : OrderedDict 반환
~~~Python
print('Ex4-3 : ', p1._asdict(), p4._asdict())
~~~

- replace() : 수정된 '새로운' 객체 변환 (왜냐하면 튜플은 불변이기 때문이다)

~~~python
print('Ex4-4 : ', p2._replace(y=100))
print('\n')
~~~

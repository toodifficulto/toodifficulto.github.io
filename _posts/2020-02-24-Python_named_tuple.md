---
layout: post
title: 파이썬 심화 데이터 모델 Named Tuple
tags:  [Python]
---

# 데이터 모델
데이터 모델은 데이터의 관계, 접근과 그 흐름에 필요한 처리 과정에 관한 추상화된 모형이다. 소프트웨어 개발과 유지, 보수의 기준이 되기 때문에 소프트웨어 공학의 중요한 이슈이다.

[위키피디아](https://ko.wikipedia.org/wiki/%EB%8D%B0%EC%9D%B4%ED%84%B0_%EB%AA%A8%EB%8D%B8)

크롤링을 하거나 웹에서 데이터를 송신 혹은 수신 할 때 데이터 잘 모델링 해서 효율적으로 처리하는 것이 중요하다.

**이떄 데이터 모델이란 개념이 필요하다.** 그리고 데이터 모델링을 효과적으로 해야 할 필요가 있다.


&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 파이썬 데이터 모델링

**파이썬에는 객체란 데이터를 추상화 한것이다.** 그리고 모든 객체는 id, type이 값을 가진다.

예를 들어, a라는 변수도 일종의 객체이다. 왜냐하면 a는 id값을 가지고 있고 type은 int라는 클라스의 객체이며 dir을 보면 속성과 메소드가 있는 것을 볼 수 있기 때문이다.

~~~python
# 아래보면 a = 7도 일종의 객체이다.
a = 7

print(id(a), type(a), dir(a))
~~~

~~~
1671227408 <class 'int'> ['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__init_subclass__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__',
'__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'as_integer_ratio', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
~~~

파이썬에는 다음과 같은 중요한 핵심 프레임워크가 있다.

**시퀀스(Sequence), 반복(Iterator), 함수(Functions), 클래스(Class)**

데이터모델 관점에서 각각 프레임워크들을 배워보자. 가장 먼저 named tuple에 대해 알아도록 하자.


&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;


# 일반적인 튜플
튜플은 아래와 같은 성질을 가지고 있다.

* ()로 표현된다.
* 요소의 생성이나 삭제, 수정이 불가능하다. (불변형)

튜플은 변하지 않는 값들을 모아놓고 사용하는 것이 편하다. 그래서 속도도 리스트보다 빠르다.

이제 일반적인 튜플을 가지고 데이터 모델링을 해보자. 점 2개 사이의 길이를 구하는 코드가 있다고 하자. 그러면 각 점은 튜플로 표현할 수 있다.

~~~python
pt1 = (1.0, 5.0)
pt2 = (2.4, 1.5)

from math import sqrt

line_leng1 = sqrt((pt2[0] - pt1[0]) ** 2  + (pt2[1] - pt1[1]) ** 2)

print('Ex1-1 : ', line_leng1)
print('\n')
~~~

그런데 위와 같이 x,y를 index 0 과 1로 표현을 하면 다른 사람들은 알아보지 못하는 문제가 생긴다. 이런 문제를 해결하기 위해 **named tuple** 을 사용한다.

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# Named tuple
파이썬에서는 대용량의 데이터를 처리할 때 객체를 사용하는 것보단 튜플 활용을 공식 레퍼런스에서 추천하고 있다. 그 중 자주 사용되는 것이 바로 Collections 라이브러리의 Named tuple이다.

위에서 사용했던 일반적인 튜플과 비교해보면 Named Tuple은 좀 더 클래스 형태와 더 가깝다는 느낌이 있다. 그리고 위에서는 x와 y를 인덱스로 접근하였지만 Named Tuple에서는 x와 y로 접근하였다.


~~~python
# 네임드 튜플 사용

from collections import namedtuple

# 네임드 튜플 선언
# 네임드 튜플 선언은 클래스 형태로 받아들인다.
Point = namedtuple('Point', 'x y')

# 클래스인가?
pt1 = Point(1.0, 5.0)
pt2 = Point(2.4, 1.5)

# 클래스 형태인것 같지만 메모리는 훨씬 적게 잡아먹는다. 또 튜플의 성질(불변)을 가지고 있다.
# 코딩을 할 때 인덱스는 실수하기가 쉽지만 x,y는 좀 더 명확하다. 또한 클래스를 사용하는 듯한 접근방식을 취한다.
line_leng2 = sqrt((pt2.x - pt1.x) ** 2  + (pt2.y - pt1.y) ** 2)

print('Ex1-2 : ', line_leng2)
print('Ex1-3 : ', line_leng1 == line_leng2)
print('\n')

# 명확하게 데이터 구조를 정리할 수 있다.
~~~

~~~
Ex1-2 :  3.7696153649941526
Ex1-3 :  True
~~~

&nbsp;
&nbsp;
&nbsp;
&nbsp;

## 네임드 튜플 선언 방법
네임드 튜플을 선언하는 방법은 총 4가지가 있다.

* 리스트 형태
* 코마로 구분
* 공백

rename속성값은 아래 예시 처럼 만약 x가 있는데 또 다시 x가 사용된다거나 예약어인 class가 있다면 rename이 True라면 파이썬이 알아서 이름을 바꿔주는 역할을 한다.

~~~python
Point1 = namedtuple('Point', ['x', 'y']) # 리스트 형태로도 받는다.
Point2 = namedtuple('Point', 'x, y') # 코마로 구분해서도 가능하다.
Point3 = namedtuple('Point', 'x y') # 공백도 가능하다.
Point4 = namedtuple('Point', 'x y x class', rename = True) # 만약 예약어와 중복이 들어가있으면? rename옵션의 경우

# x가 중복이기 때문에 알아서 rename 해라. 그리고 class는 예약어이기 때문에 알아서 rename해라 라는 의미.

print('Ex2-1 : ', Point1, Point2, Point3, Point4)
print('\n')
~~~

~~~
Ex2-1 :  <class '__main__.Point'> <class '__main__.Point'> <class '__main__.Point'> <class '__main__.Point'>
~~~

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

## 인스턴스 만들기

Named tuple은 다음과 같이 인스턴스를 만들 수 있다. x와 y를 지정해주거나 지정해주지 않아도 되고 혹은 하나만 지정해도 된다. **또는 Dictioanry를 unpacking하는 방법도 있다. 이때 dict에 **를 부텨야 한다.**


~~~python
p1 = Point1(x=10, y=35)
p2 = Point2(20, 40)
p3 = Point3(45, y=20)
p4 = Point4(10, 20, 30, 40)
# p5 = Point3(temp_dict) # 만약 알아서 해주겠지 하면 에러가 나온다.  y값이 없다라고 한다.
# 그래서 dictionary를 unpacking할때는 **를 붙여줘야 한다.
p5 = Point3(**temp_dict)

print('Ex2-2 : ', p1, p2, p3, p4, p5)
~~~

~~~
Ex2-2 :  Point(x=10, y=35) Point(x=20, y=40) Point(x=45, y=20) Point(x=10, y=20, _2=30, _3=40) Point(x=75, y=55)
~~~


&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

## 사용
Named Tuple은 인덱스로도 접근이 가능하고 (tuple의 성질을 가지고 있으므로) 인스턴스 변수처럼 접근도 가능하다.

또한 unpacking도 가능하다.

~~~python
print('Ex3-1 : ', p1[0] + p2[1])
print('Ex3-2 : ', p1.x + p2.y) # 클래스 변수 접근 방식

# Unpacking
x, y = p3
print('Ex3-3 : ', x+y)
~~~

~~~
Ex3-1 :  50
Ex3-2 :  50
Ex3-3 :  65
~~~


&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

## Named tuple 메소드

### make() : 새로운 객체 생성
리스트를 named tuple로 만들어 준다.

~~~python
temp = [52, 38] # 시퀀스형이면서 이터레이터의 특징을 가지는 리스트.

# _make() : 새로운 객체를 생성
p4 = Point1._make(temp)

print('Ex4-1 : ', p4) # 리스트를 namedtuple로 만들 때 사용한다.
~~~

&nbsp;
&nbsp;


### fields : 필드 네임 확인
namedtuple이 있을 때 무엇이 있는지 알아보는 방법

~~~python
print('Ex4-2 : ', p1._fields, p2._fields, p3._fields)
~~~

~~~
Ex4-2 :  ('x', 'y') ('x', 'y') ('x', 'y')
~~~

&nbsp;
&nbsp;

### asdict() : OrderedDict 반환
orderedDict 형태로 반환해준다.

~~~python
print('Ex4-3 : ', p1._asdict(), p4._asdict())
~~~

~~~
Ex4-3 :  {'x': 10, 'y': 35} {'x': 52, 'y': 38}
~~~

&nbsp;
&nbsp;


### replace() : 수정된 '새로운' 객체 변환
값을 수정할 수 있다. 이때 새로운 객체로 변환을 하는데 이는 튜플은 불변이기 때문이다

~~~python
print('Ex4-4 : ', p2._replace(y=100))
~~~

~~~
Ex4-4 :  Point(x=20, y=100)
~~~

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;


#실습
~~~python

from collections import namedtuple

Song1 = namedtuple('Song1', 'name, rank')
Song2 = namedtuple('Song2', ['name', 'rank'])
Song3 = namedtuple('Song3', 'name rank')

any_song = Song1('any song', 1)
banana = Song2('banana', 2)
apple = Song3('apple', 3)

print(any_song._fields)
print(any_song.name, any_song.rank)
print(banana.name, banana.rank)
print('\n')

temp = ['bombaya', 5]
bombaya = Song1._make(temp)
print(bombaya.name, bombaya.rank)
print(bombaya._asdict())

print(bombaya._replace(rank = 3))
~~~

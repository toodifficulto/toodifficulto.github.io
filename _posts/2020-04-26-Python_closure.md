---
layout: post
title: Python Closer
tags: [python]
---
Closure에 대해 바로 들어가기 전에 필요한 **배경지식** 이 있다. **함수의 중첩, 일급 객체 그리고 파이썬의 nonlocal 스코프** 이다. 일급객체는 [파이썬 일급함수](https://seunghakbae.github.io/2020/04/12/Python_higher_order_function/)에 정리해놓았다. 여기선 **함수의 중첩** 과 **파이썬의 nonlocal 스코프** 에 대해 알아보고 **closure** 를 들어가자.

[출저](https://shoark7.github.io/programming/python/closure-in-python)

&nbsp;
&nbsp;
&nbsp;

# 함수의 중첩(Function nesting)
파이썬에서는 함수도 중첩 선언이 된다. 즉, 함수 안에 다른 함수를 정의할 수 있는 것이다.

~~~Python
def greetings():
    def say_hi():
        print("Hi, everyone :)")

    say_hi()

greetings()
~~~

아래처럼 함수 안에 함수를 감싸서 내부 함수를 호출하는 것이 함수 중첩이다.
~~~
Hi, everyone :)
~~~

# 파이썬의 변수 범위(global, nonlocal)
예를 들어 다음과 같은 함수가 있다고 하자.

~~~python
z = 3

def outer(x):
    y = 10
    def inner():
        x = 1000
        return x

    return inner()

print(outer(10))
~~~

~~~
1000
~~~
**위의 함수에는 x값에 대한 코드가 2번 제시 되었다.** 처음은 함수 실행시에 받는 임의의 x이고, 다음은 inner 함수 내에서 1000으로 초기화하는 변수 x이다. 그러면 예시처럼 함수 호출 시 인자를 10으로 줬을 때 반환되는 값은 몇일까?

위는 결국 **스코프** 에 대한 이야기다.

  * inner함수 블록 안에 있는 영역은 **local scope** 라고 불린다. 로컬 영역 안의 모든 객체들은 inner의 제어 아래에 있다.

  * outer의 안에 있되, inner의 밖에 있는 영역은 **nonlocal 스코프** 라고 불린다. outer의 y변수는 inner 입장에서는 nonlocal 스코프의 변수이다.

  * outer 함수 밖의 영역은 global 스코프이다. z 변수는 global에 선언된 변수로 outer함수뿐 아니라 다른 코드나 함수에서도 참조 가능하다.


그래서 만약 nonlocal의 x를 사용하고 싶다면 **nonlocal** 이라고 선언하면 된다.
~~~Python
def outer(x):
    y = 10

    def inner():
        nonlocal x
        print(x)
        x = 1000
        return x

    return inner()

print(outer(10))
~~~

~~~
10
1000
~~~

아래와 같은 함수가 있다고 하자. 그리고 func_v1함수를 호출해보자.

~~~python
def func_v1(a):
    print(a)
    print(b)

func_v1(5)
~~~
그러면 당연히 다음과 같이 b가 선언되지 않았기 때문에 에러가 **NameError** 가 날 것이다.
~~~
NameError: name 'b' is not defined
~~~

그렇다면, b를 전역 변수로 만든 후 위의 함수를 실행하면 어떻게 될까?

~~~python
b = 10 # 전역 범위

def func_v2(a):
    print(a)
    print(b)

func_v2(5)
~~~
그러면 다음과 같이 결과가 잘 나오는 것을 볼 수 있다.
~~~
5
10
~~~

그렇다면 다음과 같은 함수는 어떻게 될까? b는 전역변수로 선언되었고 함수 내부에서도 b라는 변수가 선언되었다.
~~~python

b = 10

def func_v3(a):
    print(a)
    print(b)
    b = 5
~~~
아래 error가 뜬다. 왜냐하면, func_v3안에 변수 b가 존재하기 때문이다. 그래서 함수입장에선 내부 변수 b가 있지만 print(b) 후에 선언이 되었으므로 다음과 같은 error를 발생시켰다.
~~~
UnboundLocalError: local variable 'b' referenced before assignment       
~~~
이럴 경우, global 예약어를 사용하면 전역변수를 사용한다고 함수에 알려준다.

~~~python
b = 10 # 전역 범위

def func_v2(a):
    global b
    print(a)
    print(b)
    b = 5
    print(b)

func_v2(5)
~~~

~~~
5
10
5
~~~

&nbsp;
&nbsp;
&nbsp;

# 클로져
위의 변수 범위를 생각하고 이제 클로져를 보자. 클로져란 반환되는 내부 함수에 대해서 선언된 연결 정보를 가지고 참조하는 방식이다. 즉, 반환 당시 함수 **유효범위를 벗어난 변수 또는 메소드에 직접 접근이** 가능하다.

예를 들어, Averager라는 클래스가 있다고 하자. 그리고 이 클래스를 호출할 때 마다 인자로 숫자를 넘겨주는데 이 숫자는 \_series라는 리스트에 누적이 된다.

~~~python
# 클래스 이용
class Averager():
    def __init__(self):
        self._series = []

    def __call__(self, v):
        self._series.append(v)
        print('class >>> {} / {}'.format(self._series, len(self._series)))

        return sum(self._series) / len(self._series)

        # 인스턴스 생성
        avg_cls = Averager()

        # 누적 확인
        print("Ex3-1 - ", avg_cls(15))
        print("Ex3-2 - ", avg_cls(35))
        print("Ex3-3 - ", avg_cls(40))
~~~

결과는 다음과 같다.
~~~
Ex3-1 -  15.0
class >>> [15, 35] / 2
Ex3-2 -  25.0
class >>> [15, 35, 40] / 3
Ex3-3 -  30.0
~~~

이런 형태의 클래스를 **클로져로 만들면 어떻게 될까?**

클로저란 먼저 **자신을 둘러싼 스코프(네임스페이스)의 상태값을 기억하는 함수** 이다. **어떤 함수가 클로저이기 위해서는 다음의 세 가지 조건을 만족해야 한다.**

**1. 해당 함수는 어떤 함수 내의 중첩된 함수여야 한다.**

**2. 해당 함수는 자신을 둘러싼(enclose)함수 내의 상태값을 반드시 참조해야 한다.**

**3. 해당 함수를 둘러싼 함수는 이 함수를 반환해야 한다.**

그러면 위의 Averager 클래스를 클로져 형태로 만들어보자.

아래 closure_avg1는 위의 세 가지 조건을 만족한다.

1. closure_avg1 함수 내의 중첩된 함수이고,

2. Enclosing하는 closure_avg1 스코프의 series 라는 상태값을 참조하고 있으며,

3. 자신을 둘러싼 함수는 자신(wrapper)을 반환하고 있다!


~~~python
def closure_avg1():

    # Free Variable 영역
    series = []

    # 클로져 영역

    def averager(v):
        series.append(v)
        print("def >>> {} / {}".format(series, len(series)))

        return sum(series) / len(series)

    return averager

avg_closure1 = closure_avg1()

print(avg_closure1)
print(avg_closure1(15))
print(avg_closure1(30))
print(avg_closure1(100))
~~~
그리고 함수를 잘 살펴보면, **series 변수는 매번 함수를 호출할 때 마다 초기화 되는 것이 아니라 값이 저장되어 누적이 된다!**

**여기에서 사용되는 개념이 Free Variable이다.**

즉, Free variable은 내부 함수인 averager에서 사용이 되지만 averager 함수 밖에 그리고 closure_avg1()함수 내부에 선언이 된 변수이다. 그래서 **매번 실행 할 때마다 초기화되는 것이 아니고 값이 유지된다.**

~~~
def >>> [15] / 1
15.0
def >>> [15, 30] / 2
22.5
def >>> [15, 30, 100] / 3
48.333333333333336
~~~

실제 free variable인 변수들을 다음과 같이 출력해서 볼 수 있다.
~~~python
print(avg_closure1.__code__.co_freevars)
~~~

~~~
('series',)
~~~

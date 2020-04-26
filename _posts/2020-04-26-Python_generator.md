---
layout: post
title: Python Coroutine
tags: [python]
---

# 루틴(Routine)이란?
컴퓨터 프로그램에서 어떤 일을 담당하는 하나의 정리된 일이다. 프로그램은 크고 작은 여러가지 루틴을 조합시킴으로써 성립된다. **루틴은 메인루틴(main routine)과 서브루틴(subroutine)으로 구분하고 있다.** 메인 루틴이란 프로그램의 주요한 부분이며, 전체의 개략적인 동작 절차를 표시하도록 만들어진다. 즉, 메인 루틴에서는 몇 가지 서브루틴을 호출하고, 서브루틴에 의해서 프로그램의 세세한 실행을 행한다.

&nbsp;
&nbsp;
&nbsp;

# 코루틴
코루틴도 서브루틴처럼 기능들을 별도의 공간에 모아 놓고 있다는 점에서 유사하다. 하지만, 서브루틴의 경우에는 메인루틴에서 특정 서브루틴의 공간으로 이동한 후에 리턴에 의해 호출자로 돌아와 다시 프로세스를 진행하는데 반해 **코루틴의 경우에는 루틴을 진행하는 중간에 멈추어서 특정 위치로 돌아갔다가 다시 운래 위치로 돌아와 나머지 루틴을 수행할 수 있다.** 또 다른 차이점은 서브루틴은 진입점과 반환점이 단 하나밖에 없어 메인루틴에 종속적이지만, 코루틴은 **진입지점이 여러개이기 때문에 메인루틴에 종속적이지 않아 대등하게 데이터를 주고 받을 수 있다는 특징이 있다.**

**파이썬에서 코루틴의 특징과 흐름.**

>
파이썬에는 yield 문이라는 특수한 구문이 있다. return 처럼 동작하지만, 사실은 입력으로 동작한다.(메인루틴에 종속적이 아니라 대등한 상태이기 때문에)
next(coroutine)은 coroutine 함수의 첫번째 yield 까지 호출한다음 대기한다. 두번째 next(coroutine)을 호출하면, 첫번째 yield 다음의 나머지 부분을 수행하고 다시 돌아와 그 다음 yield 까지 호출한다. iteration 이 가능한곳까지 next 함수가 수행된 뒤에는 StopIteration 에러가 발생하게 된다.
만약 yield 문이 특정 변수에 할당된다면, 만들어진 코루틴 객체에서 coroutine.send(value)를 호출해 주어야 한다. 첫번째 coroutine 지점(yield)에 멈춰있는 상태에서 변수에 할당 되어야 하는데 아무런 값도 들어오지 않는다면 에러가 발생하게 된다. 즉, yield 를 통해서 메인루틴과 서브루틴간에 서로 값이 이동하면서 특정 로직을 수행하게 되는 것이다.

아래에는 coroutine1()이라는 함수를 만들었다. c1의 type을 보면 generator이다. 그래서 next()함수를 실행해보니 yield문이 나오기 전까지가 실행된다.

~~~python
def coroutine1():
    print(">>> coroutine started.")
    i = yield
    print(">>> coroutine received : {}".format(i))

c1 = coroutine1()

print(type(c1))

next(c1)
~~~

~~~
<class 'generator'>
>>> coroutine started.
~~~

send()를 통해 yield문에 값을 준다. 그러면 받은 i값을 print()를 통해 출력하고 다음 yield값이 없으므로 StopIteration이 발생한다.

~~~python
def coroutine1():
    print(">>> coroutine started.")
    i = yield
    print(">>> coroutine received : {}".format(i))

c1 = coroutine1()

print(type(c1))

next(c1)

c1.send(100)
~~~

~~~
>>> coroutine started.
>>> coroutine received : 100
Traceback (most recent call last):
  File "c:/Users/ASUS/Programming/Python/Python_Language/Advanced_Grammar/Chatper06_02_2_practice.py", line 12, in <module>
    c1.send(100)
StopIteration
~~~
&nbsp;
&nbsp;
&nbsp;

# 제너레이터 상태
제너레이터는 아래와 같은 상태가 있다.

1. GEN_CREATED : 처음 대기 상태
2. GEN_RUNNING : 실행 상태
3. GEN_SUSPENDED : yield 대기 상태
4. GEN_CLOSED : 실행 완료 상태

~~~python
def coroutine2(x):
    print('>>> coroutine started : {}'.format(x))
    y = yield x
    print('>>> coroutine received : {}'.format(y))
    z = yield x + y
    print('>>> coroutine received : {}'.format(z))

c3 = coroutine2(10)

from inspect import getgeneratorstate

print(getgeneratorstate(c3))

print(next(c3))

print(getgeneratorstate(c3))

print(c3.send(15))
~~~
먼저 coroutine2의 object가 만들어진 상태는 GEN_CREATED상태이다. 그리고 난 후 next()함수가 호출되고 yield x까지 실행이 된다.
그리고 난 후 다시 한번 상태를 보면 GEN_SUSPENDED이다. 그리고 나선 send()를 통해 값이 전달 되고 y값에 저장이 된다. 그리고 print()문이 실행 되고 yield x + y값이 실행된다.


~~~
GEN_CREATED
>>> coroutine started : 10
10
GEN_SUSPENDED
>>> coroutine received : 15
25
~~~

# 데코레이터 패턴
데코레이터와 코루틴을 함께 사용해보자.

> **functools.wraps는** 보통 python fucntion을 wrapping할 때 원래 function의 정보들을 유지 시키기 위해서 사용된다.
~~~python
from functools import wraps

def coroutine(func):

    @wraps(func)
    def primer(*args, **kwargs):
        gen = func()
        next(gen)
        return gen

    return primer

@coroutine
def sumer():
    total = 0
    term = 0

    while True:
        term = yield total
        total += term

su = sumer()

print(su.send(100))
print(su.send(50))
~~~

그러면 다음과 같은 결과값이 나온다.
~~~
100
150
300
~~~

&nbsp;
&nbsp;
&nbsp;

# yield from
yield를 하긴 하는데 내가 가진 값을 바로 yieldㅏ는 게 아니라 '어딘가'로부터(from) 받아온 값을 yield해준다는 의미이다. 그리고 그 '어딘가'는 바로 iterable한 또다른 객체이다.

~~~python
def gen1():
    for x in 'AB':
        yield x
    for y in range(1,4):
        yield y

t1 = gen1()


print('Ex5-1 - ', next(t1))
print('Ex5-2 - ', next(t1))
print('Ex5-3 - ', next(t1))
print('Ex5-4 - ', next(t1))
print('Ex5-5 - ', next(t1))
~~~

~~~
Ex5-1 -  A
Ex5-2 -  B
Ex5-3 -  1
Ex5-4 -  2
Ex5-5 -  3
~~~

위의 for문을를 다음과 같이 바꿀 수 있다.

~~~python
def gen2():
    yield from 'AB'
    yield from range(1,4)

t3 = gen2()

print('Ex6-1 - ', next(t3))
print('Ex6-2 - ', next(t3))
print('Ex6-3 - ', next(t3))
print('Ex6-4 - ', next(t3))
print('Ex6-5 - ', next(t3))
~~~

~~~
Ex5-1 -  A
Ex5-2 -  B
Ex5-3 -  1
Ex5-4 -  2
Ex5-5 -  3
~~~
# 출저
1. [https://devbox.tistory.com/entry/IT%EC%9A%A9%EC%96%B4-%E3%84%B9](https://devbox.tistory.com/entry/IT%EC%9A%A9%EC%96%B4-%E3%84%B9)

2. [https://itholic.github.io/python-yield-from/](https://itholic.github.io/python-yield-from/)

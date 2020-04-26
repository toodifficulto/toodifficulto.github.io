---
layout: post
title: Python decorator
tags: [python]
---

# 데코레이터란?
데코레이터를 알기 위해선 퍼스크 클래스와 클로저를 알고 있어야 한다.

[퍼스트 클래스 블로그](https://seunghakbae.github.io/2020/04/12/Python_higher_order_function/)

[클로저 블로그](https://seunghakbae.github.io/2020/04/26/Python_closure/)

**데코레이터란 무엇일까?** 이름 그대로 **기존의 코드에 여러가지 기능을 추가하는 파이썬 구문이라고 생각하면 된다.**

먼저 데코레이터를 사용하는 이유는 다음과 같다.

1. 중복제거, 코드간결
2. 클로져보다 문법 간결
3. 조합해서 사용 용이.

데예를 들어 매번 함수를 실행할 때 다음과 같이 반복한다고 하자.

print("started!")

print("ended!")

**데코레이터를 사용하면 어느 함수에서든 위 문구를 반복할 수 있다.**

예를 들어, 다음과 같은 함수가 있다고 하자.

~~~python
def outer_function(msg):
    def inner_function():
        print(msg)
    return inner_function

hi_func = outer_function('Hi')
bye_func = outer_function('Bye')

hi_func()
bye_func()
~~~
코드를 실행하면, 다음과 같다.
~~~
Hi
Bye
~~~

데코레이터 코드도 위의 코드와 아주 유사하다. **다만, 함수를 다른 함수의 인자로 전달하는 점이 조금 다르다.**

~~~python
def decorator_function(original_function):
    def wrapper_function():
        return original_function()

    return wrapper_function

def display():
    print("display 함수가 실행되었습니다.")

decorated_display = decorator_function(display)

decorated_display()
~~~
데코레이터 함수인 decorator_funciton과 일반 함수인 display를 각각 정의했다. 그 다음에 decorated_display라는 변수에 display 함수를 인자로 갖는 decorator_function을 실행한 리턴값을 할당하였다. 이 리턴값은 wrapper_function이 된다. 여기서 wrapper_function 함수는 아직 실행된것이 아니고, doecorated_display()를 통해 wrapper_function을 호출하면 정의된 wrapper_function이 호출된다. 그러면 original_function인 display함수가 호출되어 문자열이 출력이 된다.

~~~
display 함수가 실행되었습니다.
~~~

**이런 복잡한 데코레이터를 사용하는 이유는 이미 만들어져 있는 기존의 코드를 수정하지 않고도, 래퍼(wrapper)함수를 이용하여 여러가지 기능을 추가할 수 있기 때문이다.**

예를 들어,
~~~python

def decorator_function2(original_function):
    def wrapper_function():
        print("{} 함수가 호출되기 전입니다. ".format(original_function.__name__))

        return original_function()

    return wrapper_function

def display_1():
    print("display_1 함수가 실행되었습니다.")

def display_2():
    print("display_2 함수가 실행되었습니다.")

displayed_1 = decorator_function2(display_1)
displayed_2 = decorator_function2(display_2)

displayed_1()
print()
displayed_2()
~~~

~~~
display_1 함수가 호출되기 전입니다.
display_1 함수가 실행되었습니다.

display_2 함수가 호출되기 전입니다.
display_2 함수가 실행되었습니다.
~~~
위의 예제와 같이 하나의 데코레이터 함수를 만들어 display_1과 display_2, 두 개의 함수에 기능을 추가할 수 있다.

그런데 일반적으로 위의 예제와 같이 decorator를 사용하지 않고, **@심볼과 데코레이터 함수의 이름을 붙여 쓰는 간단한 구문을 사용한다.**

~~~python
def decorator_function2(original_function):
    def wrapper_function():
        print("{} 함수가 호출되기 전입니다. ".format(original_function.__name__))

        return original_function()

    return wrapper_function

@decorator_function2
def display_1():
    print("display_1 함수가 실행되었습니다.")

@decorator_function2
def display_2():
    print("display_2 함수가 실행되었습니다.")

display_1()
display_2()
~~~

~~~
display_1 함수가 호출되기 전입니다.
display_1 함수가 실행되었습니다.
display_2 함수가 호출되기 전입니다.
display_2 함수가 실행되었습니다.
~~~

그런데 만약 display_info함수와 같이 인수를 가진 함수를 데코레이팅하고 싶을 땐 어떻게 해야 할까?

~~~python
def decorator_function3(original_function):
    def wrapper_function():
        print('{} 함수가 호출되기전 입니다.'.format(original_function.__name__))
        return original_function()
    return wrapper_function


@decorator_function3
def display_3():
    print('display 함수가 실행됐습니다.')


@decorator_function3
def display_info(name, age):
    print('display_info({}, {}) 함수가 실행됐습니다.'.format(name, age))

display_3()
print()
display_info('John', 25)
~~~
그러면 다음과 같은 에러가 뜬다.
~~~
File "c:/Users/ASUS/Programming/Python/Python_Language/Advanced_Grammar/Chapter04_02_practice.py", line 113, in <module>
  display_info('John', 25)
TypeError: wrapper_function() takes 0 positional arguments but 2 were given
~~~

이런 경우 다음과 같이 수정하면 된다.

~~~python
def decorator_function3(original_function):
    def wrapper_function(*args, **kwargs):
        print('{} 함수가 호출되기전 입니다.'.format(original_function.__name__))
        return original_function(*args, **kwargs)
    return wrapper_function


@decorator_function3
def display_3():
    print('display 함수가 실행됐습니다.')


@decorator_function3
def display_info(name, age):
    print('display_info({}, {}) 함수가 실행됐습니다.'.format(name, age))

display_3()
print()
display_info('John', 25)
~~~

~~~
display_3 함수가 호출되기전 입니다.
display_3 함수가 실행됐습니다.

display_info 함수가 호출되기전 입니다.
display_info(John, 25) 함수가 실행됐습니다.
~~~

&nbsp;
&nbsp;

데코레이터는 주로 로그를 남기거나 유저의 로그인 상태등을 확인하여 로그인 상태가 아니면 로그인 페이지로 리더렉트 하기 위해서 사용되거나 성능을 측정하기 위해 사용된다.

~~~python
import time

def perf_clock(func):

    def wrapper_func(*args, **kwargs):
        # 시작시간
        st = time.perf_counter()

        result = func(*args, **kwargs)

        # 종료시간
        et = time.perf_counter()

        # 함수명
        name = func.__name__

        # 매개변수
        arg_str = ','.join(repr(arg) for arg in args)

        # 출력
        print("Result : [%0.5fs] %s (%s) -> %r" % (et, name, arg_str, result))

        return result

    return wrapper_func

@perf_clock
def sum_func(*numbers):
    return sum(numbers)

sum_func(100)
~~~

~~~
Result : [0.07621s] sum_func (100) -> 100
~~~

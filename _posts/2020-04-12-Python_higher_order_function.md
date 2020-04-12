---
layout: post
title: Python Higher-Order Function
tags: [python]
---

# 일급함수란?
일급함수란 프로그래밍 언어에서 **함수를 값으로** 다를 수 있는 것이다. (함수 스스로 객체취급)

즉, 함수를 **변수** 에 담아 원할 때 평가(함수 호출)하는 것이다.

#### 파이썬 함수의 특징
* 1. 런타임 초기화 가능(실행 시 초기화가 가능)

* 2. 변수등에 할당 가능(데코레이터, 클로져등을 사용 가능)

* 3. 함수 인수 전달 가능 sorted(keys=len) (함수 안에 함수를 전달한다.)

* 4. 함수 결과로 반환 가능 return funcs

&nbsp;
&nbsp;
&nbsp;

# 함수 객체 예제
아래 factorial이라는 함수와 A라는 class가 있다고 하자.

~~~python
def factorial(n):
    '''Factorial Function -> n:int'''
    if n == 1: # n < 2
        return 1
    return n * factorial(n-1)

class A:
    pass
~~~

~~~python
print('Ex1-1 - ', factorial(5))
print('Ex1-2 - ', factorial.__doc__)
print("Ex1-3 - ", type(factorial), type(A))
print('Ex1-4 - ', sorted(set(dir(factorial)) - set(dir(A))))
print("Ex1-5 - ", factorial.__name__) # 함수의 이름을 출력
print("Ex1-6 - ", factorial.__code__) # 파이썬 파일의 위치 인자와 코드들을 갖고 있다.
~~~

결과값은 다음과 같다. _/_name_/_ 함수의 이름을 출력하고, _/_code_/_ 는 파일의 위치, 인자 코드를 갖고 있다.

~~~
Ex1-1 -  120
Ex1-2 -  Factorial Function -> n:int
Ex1-3 -  <class 'function'> <class 'type'>
Ex1-4 -  ['__annotations__', '__call__', '__closure__', '__code__', '__defaults__', '__get__', '__globals__', '__kwdefaults__', '__name__', '__qualname__']
Ex1-5 -  factorial
Ex1-6 -  <code object factorial at 0x00E32F28, file "c:/Users/ASUS/Programming/Python/Python_Language/Advanced_Grammar/Chapter_04_01.py", line 22>
~~~

그리고 함수를 **변수** 에 할당해보자.

그리고 다음과 같이 사용이 가능하다.
~~~python
var_func = factorial

print('Ex2-1 - ', var_func)
print('Ex2-2 - ', var_func(5))
print('Ex2-3 - ', map(var_func, range(1,6))) # range값의 원소를 하나씩 var_func에 인자로 넣어준다.
print('Ex2-4 - ', list(map(var_func, range(1,6))))
~~~

~~~
Ex2-1 -  <function factorial at 0x00BA7778>
Ex2-2 -  120
Ex2-3 -  <map object at 0x02FB0490>
Ex2-4 -  [1, 2, 6, 24, 120]
~~~

또한 함수는 함수 안에 넣을 수도 있다. 예를 들어, map함수 안에 fac, 그리고 filter 함수 안에 lambda 함수를 넣었다.

~~~python
print('Ex3-1 -', list(map(var_func, filter(lambda x: x % 2, range(1,6)))))
print('Ex3-2 - ', [var_func(i) for i in range(1,6 ) if i % 2])
~~~

~~~
Ex3-1 - [1, 6, 120]
Ex3-2 -  [1, 6, 120]
~~~

&nbsp;
&nbsp;
&nbsp;

# reduce 함수
reduce함수는 권장하진 않지만, 다른 언어에서도 많이 사용되므로 알아두는 게 좋다.

reduce함수는 매개변수로 함수와 iterable을 갖는다. 그리고 iterable한 요소를 function에 대입하여 결국 하나의 결과값을 리턴해주느 함수이다.

~~~python
from functools import reduce
from operator import add

# 1 ~ 10까지 더한 값.
print('Ex3-3 - ', reduce(add, range(1,11))) # 누적
print('Ex3-4 - ', sum(range(1,11)))
~~~

&nbsp;
&nbsp;
&nbsp;

# 익명 함수(lambda)
가급적이면 주석을 함께 사용한다. 그리고 python PEP 8에 따르면 익명함수보단 함수 사용을 추천한다. 즉, 일반 함수 형태로 리팩토링을 권장한다. 하지만, 적재적소에 사용하면 매우 유용하다.

**Lambda의 정의**
> lambda 인자리스트 : 표현식
> lambda 매개변수 : 표현식

예를 들어, 10을 더해주는 함수가 있다고 하자. 이 함수를 익명함수로 만들면 다음과 같다.
~~~python
def plus_ten(x):
  return x + 10

# x는 매개변수, x + 10은 표현식
lambda x : x + 10
~~~

~~~Python
print('Ex3-5 - ', reduce(lambda x, t: x + t, range(1, 11)))
print()
print()
~~~

~~~
Ex3-5 -  55
~~~

&nbsp;
&nbsp;
&nbsp;

# Callable
Callable은 호출 연산자로 메소드 형태로 호출이 가능한지를 확인하는 함수이다.

매직 메소드로 \_\_call\_\_ 이 있는 함수나 클래스들은 callable하다. 즉, **()를 붙여서 호출이 가능하다라는 뜻이다.**

예를 들어, 다음과 같이 로또 추천 클래스를 선언했다고 하자.
~~~python
import random

# 로또 추첨 클래스 선언
class LottoGame:
    def __init__(self):
        self._balls = [n for n in range(1, 46)]

    def pick(self):
        random.shuffle(self._balls)

        return sorted([random.choice(self._balls) for n in range(6)])

    def __call__(self):
        return self.pick()

print('Ex4-3 - ', game()) # 바로 호출이 된다.
~~~

그리고 매직 메소드인 \_\_call\_\_을 수정하였다. 그래서 LottoGame()을 호출하면 \_\_call\_\_ 함수가 호출되면서 self.pick() 함수가 호출된다.

~~~
Ex4-3 -  [8, 16, 19, 23, 28, 38]
~~~

&nbsp;
&nbsp;
&nbsp;

# 다양한 매개변수 입력(\*args, \*\*kwargs)

다양한 매개변수 입력 unpacking을 다시 한번 연습해보자.

\*가 한개 있다면 packing해서 tuple형태로 매개변수가 넘어온다. \*\*는 dictionary형태로 매개변수가 넘어온다.

~~~python
def args_test(name, *contents, point=None, **attrs):
    return '<args_test> ->({}) ({}) ({}) ({})'.format(name, contents, point, attrs)

print('Ex5-1 - ', args_test('test1')) # 하나 보낼 땐 name이 받는다.
# *는 packing해서 tuple형태로 내려온다. **는 dictionary형태로 넘어온다.
print('Ex5-2 - ', args_test('test1', 'test2')) # test1은 name에 넘어가고, test2는 tuple형식으로 *contents에 넘어간다.
print('Ex5-3 - ', args_test('test1', 'test2', 'test3', id='admin')) # test1은 name이 받고, test2와 test3은 tuple로 받는다. 그리고 point가 없으므로
# id를 key값으로 하는 dictionary로 받아서 attrs에 넣는다.
print('Ex5-4 - ', args_test('test1', 'test2', 'test3', id='admin', point=7))
print('Ex5-5 - ', args_test('test1', 'test2', 'test3', id='admin', point=7, password='1234'))
~~~

그래서 하나만 받을 땐 name이 받는다. 그리고 나선 test1과 test2를 받을 땐 test1은 name에, test2는 packing되어서 tuple인 content에 넘어간다.
id가 admin인 경우 point가 아니기 때문에 key값이 id이고 value가 admin인 dictionary형태로 \*\*attrs에 넘어간다.

~~~python
Ex5-1 -  <args_test> ->(test1) (()) (None) ({})
Ex5-2 -  <args_test> ->(test1) (('test2',)) (None) ({})
Ex5-3 -  <args_test> ->(test1) (('test2', 'test3')) (None) ({'id': 'admin'})
Ex5-4 -  <args_test> ->(test1) (('test2', 'test3')) (7) ({'id': 'admin'})
Ex5-5 -  <args_test> ->(test1) (('test2', 'test3')) (7) ({'id': 'admin', 'password': '1234'})
~~~

&nbsp;
&nbsp;
&nbsp;

# 함수 Signature
Signature 함수는 함수 인자에 대해 알려주는 메소드이다.

~~~python
sg = signature(args_test)

print('Ex6-1 - ', sg) # 함수의 파라미터 값을 보여준다.
print('Ex6-1 - ', sg.parameters)

# 모든 정보 출력
for name, param in sg.parameters.items():
    print('Ex6-3 - ', name, param.kind, param.default)
~~~

~~~
Ex6-1 -  (name, *contents, point=None, **attrs)
Ex6-1 -  OrderedDict([('name', <Parameter "name">), ('contents', <Parameter "*contents">), ('point', <Parameter "point=None">), ('attrs', <Parameter "**attrs">)])
~~~

&nbsp;
&nbsp;
&nbsp;

# partial 함수
partial 함수는 **인수를 고정시킨다.** 그래서 주로 특정 인수를 고정 후 콜백 함수에 사용한다.

> 하나 이상의 인수가 이미 할당된(채워진) 함수의 새 버전 반환

> 함수의 새 객체 타입은 이전 함수의 자체를 기술하고 있다.

그래서 partial함수를 통해 mul함수와 인자 5를 고정시켜 five라는 함수를 만든다. six는 five에 인자 하나를 더 고정시킨다.

~~~python
from operator import mul
from functools import partial

print('Ex7-1 - ', mul(10, 100))

# 인수 고정
five = partial(mul, 5) # 이것은 인수 하나를 5로 고정시킨다는 의미이다.

# 고정 추가
six = partial(five, 6)

print('Ex7-2 - ', five(100))
# print('Ex7-2 - ', five(100, 100)) # argument값이 하나만 있어야 되므로 에러가 난다.

print('Ex7-3 - ', six())

# 유용하게 많이 사용되는 함수이다.
print('Ex7-4 - ', [five(i) for i in range(1,11)])
print('Ex7-5 - ', list(map(five, range(1, 11))))
~~~

~~~
Ex7-1 -  1000
Ex7-2 -  500
Ex7-3 -  30
Ex7-4 -  [5, 10, 15, 20, 25, 30, 35, 40, 45, 50]
Ex7-5 -  [5, 10, 15, 20, 25, 30, 35, 40, 45, 50]
~~~

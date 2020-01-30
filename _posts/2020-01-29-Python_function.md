---
layout: post
title: 파이썬의 함수와 람다
tags:  [python]
---

# 함수 사용하는 이유
반복적인 일을 함수로 작성한다. 하나의 기능을 하나의 함수로 만드는 것이 가장 좋다. 그래서 여러 가지 함수를 만들어 놓고 쉽게 구분할 수 있다.

&nbsp;
&nbsp;
&nbsp;

# 함수사용
### 함수 정의 방법
~~~python
def 함수명 (parameter):
    code
~~~

**예시**
~~~python
def hello_return(world):
    val = "Hello " + str(world)
    return val
~~~

&nbsp;
### 함수호출
~~~python
함수명(parameter)
~~~

**예시**
~~~python
strs = hello_return('python!')
print(strs)
~~~

먼저 함수를 선언하고 함수는 함수 선언 밑에 호출해야 한다.

~~~python
def hello(world):
    print('Hello', world)

hello('python')
hello(777)
~~~

만약 아래처럼 호출을 먼저하고 함수를 선언하면 error가 난다.
~~~python
함수 선언 위에 호출하면 error가 난다.

hello('python')
hello(777)

def hello(world):
    print('Hello', world)
~~~
~~~
File "<ipython-input-3-e3bbf011bdab>", line 1
    함수 선언 위에 호출하면 error가 난다.
        ^
SyntaxError: invalid syntax
~~~

&nbsp;
&nbsp;
&nbsp;

# 다중리턴
여러 값을 리턴할 경우를 다중리턴이라고 한다.

~~~python
def func_mul(x):
    y1 = x * 100
    y2 = x * 200
    y3 = x * 300
    return y1, y2, y3

# 받는 쪽에선 3개가 return 되기 떄문에 3개를 받아야 한다.
val1, val2, val3 = func_mul(100)
print(val1, val2, val3)

# 만약 list로 받고 싶다면?
# 데이터 타입 반환

def func_mul_2(x):
    y1 = x * 100
    y2 = x * 200
    y3 = x * 300
    return [y1, y2, y3]

lt = func_mul_2(100)
print(lt, type(lt))
~~~

&nbsp;
&nbsp;
&nbsp;

# *args, *kwargs

**args**
는 매개변수가 몇 개가 들어올지 모를때, 혹은 다양한 매개변수를 받을 때 **tuple형식** 으로 매개변수를 받는다.

**kwargs**
는 매개변수를 **dictionary형태** 로 받는다.

~~~python
# 매개변수가 몇 개가 들어오는 지 모를때
# 다양한 매개변수를 받을 때
# tuple 형태로 다 바꾼다.
# tuple이므로 iterator형태로 사용이 가능하다.
def args_func(*args):
    print(args)
    for t in args:
        print(t)

args_func('kim')
args_func('kim', 'park')
args_func('kim', 'Park', 'Lee')

# emulator
def args_func(*args):

# enumerate를 사용하면 index를 함께 보여준다.
    for i, v in enumerate(args):
        print(i, v)

args_func('kim')
args_func('kim', 'park')
args_func('kim', 'Park', 'Lee')
~~~

~~~
('kim',)
kim
('kim', 'park')
kim
park
('kim', 'Park', 'Lee')
kim
Park
Lee
0 kim
0 kim
1 park
0 kim
1 Park
2 Lee
{'name1': 'Kim', 'name2': 'Park', 'name3': 'Lee'}
~~~

~~~python
# *가 하나일 때는 tuple로 받는데, **일때는 dictionary로 받는다.
# kwargs
def kwargs_func(**kwargs):
    print(kwargs)

kwargs_func(name1='Kim', name2='Park', name3='Lee')

def kwargs_func(**kwargs):
    for k,v in kwargs.items():
        print(k, v) # key와 value print

kwargs_func(name1='Kim', name2='Park', name3='Lee')
~~~~

~~~
{'name1': 'Kim', 'name2': 'Park', 'name3': 'Lee'}
name1 Kim
name2 Park
name3 Lee
~~~

주로 args와 kwargs를 혼합하여 아래처럼 사용한다.

~~~python

# *args는 tuple형태, **kwargs는 dictionary형태

def example_mul(arg1, arg2, *args, **kwargs):
    print(arg1, arg2, args, kwargs)

example_mul(10,20)
example_mul(10,20, 'park', 'kim')
example_mul(10,20, 'park', 'kim', age1=24, age2=35)
~~~

~~~
10 20 () {}
10 20 ('park', 'kim') {}
10 20 ('park', 'kim') {'age1': 24, 'age2': 35}
~~~

&nbsp;
&nbsp;
&nbsp;


# 중첩함수(클로저)
중첩함수는 함수 안에 또 함수를 선언하는 것이다.

~~~python
def nested_func(num):
    # 함수 안에 함수가 있다.
    def func_in_func(num):
        print('>>>>',num)

    print('in func')
    func_in_func(num + 10000)

nested_func(10000)
~~~

~~~
in func
>>>> 20000
~~~

# 힌트
파이썬 함수에는 힌트라는 것이 있다.
input이 어떤 type이고 반환 값이 어떤 type인지를 알려주는 역할을 한다.

~~~python
# 예제 6 (
# 파이썬에는 힌트라는 것이 있다.
# 이 x는 무엇이어야 돼 그리고 리스트가 반환이 돼 라는 것을
# 알려준다.
def func_mul3(x : int) -> list:
    y1 = x * 100
    y2 = x * 200
    y3 = x * 300
    return [y1, y2, y3]

print(func_mul3(5.0)) # 예외를 발생시키지는 않는다.
print(func_mul3(5))
~~~
~~~
[500.0, 1000.0, 1500.0]
[500, 1000, 1500]
~~~

&nbsp;
&nbsp;
&nbsp;

# 람다 함수
람다식 쓰는 이유 : 메모리 절약, 가독성 향상, 코드 간결

함수는 객체 생성 -> 리소스(메모리) 할당

람다는 즉시 실행(Heap 초기화) -> 메모리 초기화

#### 사용방법
lambda함수 옆에 input값을 넣고 : 뒤에는 output값을 넣는다.

~~~python
lambda input : output
~~~

~~~python
# 일반적 함수 -> 변수 할당
def mul_10(num : int) -> int:
    return num * 10

var_func = mul_10
print(var_func) # 이 함수는 사용하진 않았지만, 객체가 생성이 되어 메모리에 벌써 올라가 있다.

print(type(var_func))

print(var_func(10))

# 람다를 사용할 때는 주로 익명함수를 사용할 때 사용
lambda_mul_10 = lambda num:num * 10 #이 num가 의미하는 게 input이고, 출력은 num * 10으로 나가라는 의미

print('>>>',lambda_mul_10(10))

# 매개변수로 함수도 받을 수 있다.
def func_final(x,y, func):
    print(x * y * func(10)) #그리고 함수를 여기서 호출한다.

func_final(10, 10, lambda_mul_10)

func_final(10, 10, lambda x : x * 10)

# 함수를 매개변수로 넘길 때 lamda함수로 작성해서 넘길때도 사용된다.
~~~

~~~
<function mul_10 at 0x0000028993962620>
<class 'function'>
100
>>> 100
10000
10000
~~~

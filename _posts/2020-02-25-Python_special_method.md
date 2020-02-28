---
layout: post
title: 파이썬 Magic Method Advanced
tags:  [Python]
---

# 매직 메소드란?
* 클래스안에서 정의할 수 있는 스페셜 메소드이며 클래스를 int, str, list등의 파이썬의 빌트인 타입(built-in type)과 같은 작동을 하게 해준다.

* +, -, >, <등의 오퍼레이션에 대해서 각각의 데이터 타입에 맞는 메소드로 오버로딩하여 백그라운드에서 연산을 한다.

* \_\_init\_\_ 이나 \_\_str\_\_과 같이 메소드 이름 앞뒤에 더블 언더스코어("\_\_")를 붙인다.

[참고 사이트](http://schoolofweb.net/blog/posts/%ED%8C%8C%EC%9D%B4%EC%8D%AC-oop-part-6-%EB%A7%A4%EC%A7%81-%EB%A9%94%EC%86%8C%EB%93%9C-magic-method/)

예를 들어,
~~~python
print(dir(int))
~~~

int class의 dir을 보면 아래와 같은 매직 메소드가 있다.
~~~
['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__init_subclass__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'as_integer_ratio', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
~~~

그리고 잘 살펴보면 \_\_add\_\_ 라는 함수나 \_\_bool\_\_, 혹은 \_\_mul\_\_ 같은 함수도 존재한다.

위 함수들은 다음과 같다. **즉 사칙연산을 할 때 사용하는 +, -, %, / 등은 사실 매직메소드였던 것이다!**

~~~python
n = 100

print(n + 200)
print(n.__add__(200))
print(n * 200)
print(n.__mul__(200))
~~~

~~~
300
300
20000
20000
~~~

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 매직 메소드 사용 예시
이제 실제 매직메소드를 사용하는 예시를 살펴보자.

Singer 클래스는 object 클래스를 상속받는다.
그리고 object 클래스에는 \_\_str\_\_라는 함수, \_\_ge\_\_라는 함수, \_\_le\_\_ 함수, \_\_sub\_\_함수가 존재한다.

이들을 오버라이딩해서 내가 원하는 방식으로 내용을 바꿔주면 된다.

~~~python
class Singer(object):

    '''
    Author Name : Bae
    Class Description : Singer class showing rank and name of singer
    '''

    # instance method
    def __init__(self, name, rank):
        self._name = name
        self._rank = rank

    # 매직 메소드
    '''shows name and rank'''
    def __str__(self):
        return 'name : {} rank : {}'.format(self._name, self._rank)

    # 매직 메소드
    def __ge__(self, x):
        print('Called >> __ge__ method')
        if self._rank >= x._rank:
            return True
        else:
            return False

    # 매직 메소드
    def __le__(self, x):
        print('Called << __le__ method')
        if self._rank <= x._rank:
            return True
        else:
            return False

    def __sub__(self, x):
        print('Called - __sub__ method')
        return self._rank - x._rank

    # 클래스 메소드
    @classmethod
    def const(cls, name, rank):
        return cls(name, rank)
~~~

이제 instance화 해주자.

~~~python
iu = Singer.const('IU', 1)
zico = Singer.const('Zico', 2)
jannabi = Singer('jannabi', 3)

print(iu, zico, jannabi)
~~~

그러면 아래와 같이 \_\_str\_\_함수가 잘 반영 된것을 볼 수 있다.
~~~
name : IU rank : 1 name : Zico rank : 2 name : jannabi rank : 3
~~~

또한 \_\_le\_\_ 메소드나 \_\_sub\_\_ 메소드, 그리고 \_\_ge\_\_메소드도 잘 반영된 것을 볼 수 있다.

~~~python
print(iu >= zico)
print(iu <= zico)
print(iu - jannabi)
~~~

~~~
Called >> __ge__ method
False
Called << __le__ method
True
Called - __sub__ method
-2
~~~

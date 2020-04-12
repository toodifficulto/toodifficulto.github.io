---
layout: post
title: Python MagicMethod
tags: [python]
---

# Magic Method란?
* 클래스안에 정의할 수 있는 스페셜 메소드이며 클래스를 int, str, list등의 파이썬의 빌트인 타입과 같은 작동을 하게 해준다.
* __init__ 이나 __str__ 과 같이 메소드 이름 앞뒤에 더블 언더스코어("_\_")를 붙인다.

> 클래스를 만들 때 항상 사용하는 _\_init__ 이나 _\_str__ 는 가장 대표적인 매직 메소드이다.

예를 들어, dir(int)를 보면 int 클래스 안의 매직메소드들을 볼 수 있다.
~~~python
print(dir(int))
~~~

~~~
<class 'int'>
['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__init_subclass__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'as_integer_ratio', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
~~~

&nbsp;
&nbsp;
&nbsp;

### 클래스 예제
Student라는 클래스를 만들고 매직 메소드인 _/_str__ 와 비교연산자인 _/_ge__, _/_le__, _/_sub__ 을 재정의 해보자.

~~~python
# 클래스 예제
class Student:

    def __init__(self, name, height):
        self._name = name
        self._height = height

    # 매직 메소드
    def __str__(self):
        return 'Student Class Info : {} , {}'.format(self._name, self._height)

    def __ge__(self, x):
        print('Called. >> __ge__ Method.')
        if self._height >= x._height:
            return True
        else:
            return False


    def __le__(self, x):
        print('Called. >> __le__ Method.')
        if self._height <= x._height:
            return True
        else:
            return False

    def __sub__(self, x):
        print('Called. >> __sub__ Method.')
        return self._height - x._height


# 인스턴스 생성
s1 = Student('James', 181)
s2 = Student('Mie', 165)

# 매직메소드 출력
# 객체끼리 어떻게 비교? 가능하다!
print('Ex2-1 : ', s1 >= s2)
print('Ex2-2 : ', s1 <= s2)
print('Ex2-3 : ', s1 - s2)
print('Ex2-4 : ', s2 - s1)
print('Ex2-5 : ', s1)
print('Ex2-6 : ', s2)
print('\n')
~~~

~~~
Called. >> __ge__ Method.
Ex2-1 :  True
Called. >> __le__ Method.
Ex2-2 :  False
Called. >> __sub__ Method.
Ex2-3 :  16
Called. >> __sub__ Method.
Ex2-4 :  -16
Ex2-5 :  Student Class Info : James , 181
Ex2-6 :  Student Class Info : Mie , 165
~~~

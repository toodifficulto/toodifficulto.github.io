---
layout: post
title: Python Reference
tags: [python]
---

# 함수 인자 전달 방식
프로그래밍 언어에서 일반적으로 사용하는 함수 인자 전달 방식은 2가지이다.

__1. Call by value : 값에 의한 호출__
__2. Call by reference : 참조(주소)에 의한 호출__

[참조 사이트](https://lee-seul.github.io/concept/python/2018/05/02/python-call-by-object-reference.html)

#### 1. Call by value
Cally by value는 말 그대로 값에 의한 호출, __즉 값이 지정된 변수를 인자로 넘기는 것이 아니라 변수에 할당된 값만 복사해서__ 함수의 인자로 넘긴다.

#### 2. Call by Reference
Call by reference는 call by value와는 다르게 함수의 인자로 넘어간 값이 함수 내부에서 변경이 되면 실제로 값이 변경된다.

즉, 참조를 호출하는 방식인데 C언어에서는 포인터를 통해 구현이 가능하다. 포인터는 주소 값을 가지고 있어서 포인터를 함수의 인자로 넘기면 실제 메모리에 저장되어 있는 변수의 주소값을 넘기는 것과 같아 진다.

&nbsp;
&nbsp;
&nbsp;

# Python의 방식, Call by object reference
python은 위에서 설명한 일반적인 함수 인자 전달 방식과는 조금 다른 형태를 취한다.

python에서 모든 것은 __객체(object)__ 라는 말이 있다. 1,2 등의 숫자부터 시작해 모든 것이 객체이며 __하나의 개체는 단 하나의 인스턴스로 존재한다.(싱글톤)__

~~~python
a = 2
~~~
python언어의 특성으로 인해 a = 2 라고 변수를 할당하면 2라는 값이 a가 가리키는 메모리 공간에 할당되는 것이 아니라 2라는 인스턴스의 주소값을 a가 가리키게 된다.

**즉, a = 2라고 하면 2라는 값이 a에 할당되는 것이 아니라 2라는 인스턴스를 a가 가리키게 된다.**

추가로 python에서는 reference count라는 값을 가지고 있어서 자신을 가리키는 변수의 숫자를 저장하고 있다. 예를 들어, a = 2일 때 2라는 인스턴스의 reference count는 1이다. 하지만 a += 1이 실행되면 2라는 reference count는 0이 되고 3의 인스턴스의 reference count는 1이 된다.

~~~python
a = 1
print(sys.getrefcount(1))
b = 1
print(sys.getrefcount(1))

a = [1,2,3]
print(sys.getrefcount(a))

b = a
print(sys.getrefcount(a))
~~~
보면 1씩 증가하는 것을 볼 수 있다.
~~~
128
129
2
3
~~~

&nbsp;
&nbsp;
&nbsp;

# is와 ==의 차이
x와 y를 보자. x와 y의 id값은 서로 같다. 왜냐하면 위에서 설명한 것 처럼 파이썬에서는 모든 것이 객체이다. 그래서 아래에서 만든 {'name' ...}도 하나의 객체이고 변수 x와 y는 이 객체를 각각 가리키고 있기 때문에 같은 id값을 가지고 있다. 그래서 실제 class라는 값을 추가하였을 때도
x와 y 둘다 바뀐것을 볼 수 있다.

~~~python
import sys

x = {'name' : 'kim', 'age' : 33, 'city' : 'Seoul'}
y = x

print('x : ', id(x), ' y : ', id(y))
print('x == y', x == y)
print('x is y', x is y)

x['class'] = 10
print('x : ',x, 'y : ', y)
~~~

~~~
x :  24671808  y :  24671808
x == y True
x is y True
x :  {'name': 'kim', 'age': 33, 'city': 'Seoul', 'class': 10} y :  {'name': 'kim', 'age': 33, 'city': 'Seoul', 'class': 10}
~~~


그리고 **is** 는 두 **id값** 을 비교한다. **==** 는 두 내용이 같은 지를 비교한다.

예를 들어, z라는 새로운 dict 객체를 만들었다고 하자.

x와 z는 서로 다른 객체이므로 id값은 달라서 is는 False이지만 내용은 같으므로 ==는 True이다.

~~~python
# dictionary는 dict 생성자를 사용해서 만드는 습관을 만들자.
z = {'name' : 'kim', 'age' : 33, 'city' : 'Seoul', 'class' : 10}

print('Ex2-6 - ', x, z)
print('Ex2-7 - ', x is z) # x와 z는 id값이 다르기 때문에 다르다. # 같은 객체
print('Ex2-8 - ', x is not z)
print('Ex2-9 - ', x == z) # 값이 같다.
~~~

&nbsp;
&nbsp;
&nbsp;

# Deep Copy와 Copy
객체의 복사는 얕은 복사(shallow copy)와 깊은 복사(deep copy)로 나뉜다.

예를 들어, 다음과 같은 basket class가 있다고 하자.

~~~python
class Basket:
    def __init__(self, products=None):
        if products is None:
            self._products = []
        else:
            self._products = list(products)

    def put_prod(self, prod_name):
        self._products.append(prod_name)

    def del_prod(self, prod_name):
        self._products.remove(prod_name)
~~~

그리고 basket1, basket2 그리고 basket3라는 객체를 각각 만든다.
~~~python
# copy 패키지를 사용한다.
import copy

basket1 = Basket(['Apple', 'Bag', 'Tv', 'Snack'])
basket2 = copy.copy(basket1)
basket3 = copy.deepcopy(basket1)
~~~

각 객체의 id값을 체크해보자.

~~~python
print("basket1 id : ", id(basket1)," basket2 id : ", id(basket2), " bakset3 id : ", id(basket3))
~~~

각 객체의 id값은 다르다.
~~~
basket1 id :  56944600  basket2 id :  58907400  bakset3 id :  59051000
~~~

그렇다면 각 객체 내부에 있는 내용들의 id값은 어떨까?

~~~python
print("basket1 _products id : ", id(basket1._products)," basket2 products id : ", id(basket2._products), " bakset3 products id : ", id(basket3._products))
~~~

shallow copy를 한 basket2의 \_products와 basket1의 \_products의 id값은 동일하지만, bakset3의 \_products의 id값은 다르다.  
~~~
basket1 _products id :  50931272  basket2 products id :  50931272  bakset3 products id :  50863816
~~~

__즉, deep copy로 복사한 객체는 안에 있는 속성값들도 다 다르게 복사되는 반면, shallow copy로 복사한 객체는 안에 있는 속성값들은 예전 객체의 속성값들을 그대로 이어받는다.__

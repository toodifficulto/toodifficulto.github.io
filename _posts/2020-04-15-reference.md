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

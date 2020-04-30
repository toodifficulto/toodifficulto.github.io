---
layout: post
title: Python Iteratable and Iterator
tags: [python]
---

# Iterable(iterable)
iterable에 대한 python docs의 정의는 다음과 같다.

> **Iterable**
An object capable of returning its members one at a time. Examples of iterables include all sequence types (such as list, str, and tuple) and some non-sequence types like dict and file and objects of any classes you define with an \_\_iter\_\_() or \_\_getitem\_\_() method. Iterables can be used in a for loop and in many other places where a sequence is needed (zip(), map(), ...). When an iterable object is passed as an argument to the built-in function iter(), it returns an iterator for the object. This iterator is good for one pass over the set of values. When using iterables, it is usually not necessary to call iter() or deal with iterator objects yourself. **The for statement does that automatically for you, creating a temporary unnamed variable to hold the iterator for the duration of the loop.** See also iterator, sequence, and generator.

iterable은 자신에 속한 member를 한 번에 하나씩 반환할 수 있는 object를 의미한다. **iterable** 의 대표적인 예가 모든 sequence type인 **list**, **str** 그리고 **tuple** 이다. 그리고 non-sequence type인 **dict** 와 **file** 그리고 매직 메소드인 \_\_iter\_\_()나 \_\_getitem\_\_()등이 정의된 클래스들도 **iterable** 이다.

예를 들어, temp 라는 list는 iterable하기 때문에 순차적으로 member를 불러서 사용이 가능하다.

~~~Python
temp = [1,2,3,4,5]

for i in temp:
    print(i)
~~~

~~~
1
2
3
4
5
~~~

위에서 언급한것처럼 non-sequence type인 dict이나 file도 iterable하다.

~~~python
dict_test = {'a' : 1, 'b' : 2, 'c' : 3}

for i in dict_test:
    print(i)
~~~

~~~
a
b
c
~~~

또한 iterable은 for loop 말고도, zip(), map()과 같이 sequence한 특징을 필요로 하는 작업에 유용하게 사용된다. zip()이나 map()함수의 경우 iterable을 argument로 받는 것으로 정의되어 있다.

> **zip([iterable, ...])**
This function returns a list of tuples, where the i-th tuple contains the i-th element from each of the argument sequences or iterables.

> **map(function, iterable, ...)**
Apply function to every item of iterable and return a list of the results.

~~~python
temp_1 = list(range(1,5))
temp_2 = tuple(range(1,5))

print(temp_1)
print(temp_2)

for a,b in zip(temp_1, temp_2):
    print(a, b)
~~~

~~~
[1, 2, 3, 4]
(1, 2, 3, 4)
1 1
2 2
3 3
4 4
~~~

map에 iterable 사용한 예시는 다음과 같다.

~~~python
for i in map(int, '123'):
    print(i)
~~~

~~~
1
2
3
~~~

&nbsp;
&nbsp;
&nbsp;

# iterator
iterator에 대한 python docs의 정의는 다음과 같다.

> **Iterator**
An object representing a stream of data. Repeated calls to the iterator’s next() method return successive items in the stream. **When no more data are available a StopIteration exception is raised instead.** At this point, the iterator object is exhausted and any further calls to its next() method just raise StopIteration again. Iterators are required to have an \_\_iter\_\_() method that returns the iterator object itself so every iterator is also iterable and may be used in most places where other iterables are accepted. One notable exception is code which attempts multiple iteration passes. A container object (such as a list) produces a fresh new iterator each time you pass it to the iter() function or use it in a for loop. Attempting this with an iterator will just return the same exhausted iterator object used in the previous iteration pass, making it appear like an empty container.

Iterator는 next()메소드로 데이터를 순차적으로 호출가능한 object이다. 만약 next()로 다음 데이터를 불러 올 수 없는 경우(가장 마지막 데이터인 경우) StopIteration exception이 발생한다.

그렇다면, iterable한 object들은 iterator인가? iterable이라고 해서 반드시 iterator라는 것은 아니다.

~~~python
temp = [1,2,3,4]

print(next(temp))
~~~

list는 iterable 이지만, 위와 같이 next() 메소드로 호출해도 동작하지 않는다. **만약 iterable을 iterator로 변환하고 싶다면 iter() 라는 built-in function을 사용하면 된다.**

~~~
ter_06_01_2_practice.py", line 3, in <module>
    print(next(temp))
TypeError: 'list' object is not an iterator
~~~

아래와 같이 **iter** 함수를 사용하면 iterable한 object를 iterator로 바꿔줄 수 있다.
~~~python
temp = [1,2,3,4]

print(type(temp))

iter_temp = iter(temp)

print(type(iter_temp))
~~~

~~~
<class 'list'>
<class 'list_iterator'>
~~~

그런 다음, next()함수를 통해 하나씩 꺼내거나 for문을 사용할 수 있다.

~~~Python
temp = [1,2,3,4]

print(type(temp))

iter_temp = iter(temp)

print(type(iter_temp))

print(next(iter_temp))
print(next(iter_temp))
print(next(iter_temp))
print(next(iter_temp))
~~~

~~~
<class 'list'>
<class 'list_iterator'>
1
2
3
4
~~~

# 출저
1. [https://bluese05.tistory.com/55](https://bluese05.tistory.com/55)

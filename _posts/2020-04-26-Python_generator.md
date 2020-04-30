---
layout: post
title: Python Generator
tags: [python]
---

# Generator란
generator란 간단하게 설명해서 iterator를 생성해주는 function이다. iterator는 next()메소드를 이용해 데이터에 순차적으로 접근이 가능한 object 이다.
generator란 간단하게 설명해서 iterator를 생성해주는 function이다. iterator는 next()메소드를 이용해 데이터에 순차적으로 접근이 가능한 object 이다. 제너레이터는 배열이나 리스트를 리턴하는 함수와 비슷하며 호출을 할 수 있는 파라미터를 가지고 있으며 연속적인 값들을 만드렁 낸다.

**하지만, 한번에 모든 값을 포함한 배열을 만들어서 리턴하는 대신에 (list는 이 경우에 속한다.) yield 구문을 이용해 한 번 호출할 때 마다 하나의 값만을 리턴하고 이런 이유로 일반 반복자에 비해 아주 작은 메모리를 필요로 한다.**

또한 일반 함수가 호출되면 코드의 첫 번째행 부터 시작하여 리턴(return) 구문이나 예외(exception) 또는 (리턴을 하지 않는 함수이면) 마지막 구문을 만날때까지 실행된 후, 호출자(caller)에게 모든 콘트롤을 리턴한다. 그리고 함수가 가지고 있던 모든 내부 함수나 모든 로컬 변수는 메모리상에서 사라진다.

그러나 한번에 일을 다하고 영원히 사라져버리는 함수가 아닌 하나의 일을 마치면 했던 일을 기억하면서 대기하고 있다가 다시 호출되면 전의 일을 계속 이어져 하는 함수가 필요해졌다. 그것이 바로 **제너레이터** 이다. 제너레이터를 사용하면 일반 함수보다 훨씬 좋은 퍼포먼스를 낼 수가 있고, 메모리 리소스도 절약할 수 있다.

즉 요약하면 아래와 같다.
1. 지능형 리스트, 딕셔너리, 집합 -> 데이터 셋이 증가 될 경우 메모리 사용량 증가 -> 제너레이터 완화

2. 단위 실행 가능한 코루틴(Coroutine) 구현에 아주 중요하다.

3. 딕셔너리, 리스트 한 번 호출 할 때 마다 하나의 값만 리턴 -> 아주 작은 메모리 양을 필요로 한다.

~~~python
def square_numbers(nums):
    result = []

    for i in nums:
        result.append(i * i)

    return result

my_nums = square_numbers([1,2,3,4,5])

print(my_nums)
~~~
인자로 받은 리스트를 for 루프를 돌면서 i * i의 결과값으로 새로운 리스트를 만들고 리턴하는 함수이다.
~~~
[1, 4, 9, 16, 25]
~~~

이 코드를 제너레이터로 만들어 보자.

~~~python
def square_numbers(nums):
    for i in nums:
        yield i * i

my_nums = square_numbers([1,2,3,4,5])

print(my_nums)
~~~

제너레이터라는 오브젝트가 반환되었다. **제너레이터는 리턴할 모든 값을 메모리에 저장하지 않기 때문에 조금 전 일반 함수의 결과와 같이 한번에 리스트로 보이지 않는다.** 제너레이터는 한 번 호출될때마다 **하나의 값만을 전달(yield)한다.**

~~~
<generator object square_numbers at 0x0395A728>
~~~
iterator인 generator는 next함수를 통해 다음값을 볼 수 있다.
~~~python
print(next(my_nums))
print(next(my_nums))
print(next(my_nums))
print(next(my_nums))
print(next(my_nums))
print(next(my_nums))
~~~
next()함수를 통해 잘 반환하다가 모든 숫자가 다 반환된 후 next()함수가 또 호출되면 StopIteration 에러가 난다.
~~~
1
4
9
16
25
Traceback (most recent call last):
  File "c:/Users/ASUS/Programming/Python/Python_Language/Advanced_Grammar/Chapter_06_01_2_practice.py", line 32, in <module>
    print(next(my_
~~~

for루프를 사용하면 StopIteration이 발생하지 않는다.

~~~python
def square_numbers(nums):
    for i in nums:
        yield i * i

my_nums = square_numbers([1,2,3,4,5])

print(my_nums)

for num in my_nums:
    print(num)
~~~

~~~
<generator object square_numbers at 0x01BEA728>
1
4
9
16
25
~~~

위의 함수는 list comprehension을 사용하면 더 쉽게 만들 수 있다.
~~~Python
my_nums = [x*x for x in [1,2,3,4,5]]

for num in my_nums:
  print(num)
~~~

~~~
1
4
9
16
25  
~~~

위 구문을 조금만 바꾸면 **제너레이터를 만들 수 있다.[]을 ()로 바꾸면 된다.**

~~~python
my_nums = (x*x for x in [1,2,3,4,5])

print(type(my_nums))

print(next(my_nums))
print(next(my_nums))
print(next(my_nums))
print(next(my_nums))
~~~

~~~
<class 'generator'>
1
4
9
16
~~~

**다른 예제**
아래에는 generator_ex1()이라는 generator를 만들었다. 그리고 next()를 통해 보면 yield문까지 진행된 것을 볼 수 있다.
~~~python
def generator_ex1():
    print('start')
    yield 'AAA'
    print('continue')
    yield 'BBB'

print(type(generator_ex1()))

print(next(generator_ex1()))
~~~

~~~
<class 'generator'>
start
AAA
~~~

&nbsp;
&nbsp;
&nbsp;

# 자주 사용하는 함수

**itertools.count**
itertools.count 함수는 1부터 2.5씩 값을 더해서 무한으로 반환시켜주는 generator를 만들어주는 함수이다.

~~~python
import itertools

gen1 = itertools.count(1, 2.5)

print(next(gen1))
print(next(gen1))
print(next(gen1))
~~~

~~~
1
3.5
6.0
~~~

**itertools.takewhile**
itertools.takewhile은 주어진 함수의 조건만큼의 generator를 만들어 준다.

~~~python
import itertools

gen2 = itertools.takewhile(lambda n : n < 1000, itertools.count(1, 2.5))

for v in gen2:
    print(v)
~~~

**누적 합계 accumulate**
아래 itertools.accumulate은 주어진 값의 누적 합을 보여준다.

~~~python
# 누적 합계
gen4 = itertools.accumulate([x for x in range(1, 101)])

for v in gen4:
    print('Ex6-7 - ', v)
~~~

~~~
....
Ex6-7 -  4753
Ex6-7 -  4851
Ex6-7 -  4950
Ex6-7 -  5050
~~~

**itertools.chain()**
itertools.chain을 사용하면 다음과 같이 이어준다.
~~~python
import itertools

gen5 = itertools.chain('ABCDE', range(1, 11, 2))

print(list(gen5))
~~~

~~~
['A', 'B', 'C', 'D', 'E', 1, 3, 5, 7, 9]
~~~

**itertools.product**
product를 사용하여 모든 경우의 수를 출력할 수도 있다.

~~~python
import itertools

gen8 = itertools.product('ABCDE', repeat=2)

print('Ex6-11 - ', list(gen8))
~~~

~~~
Ex6-11 -  [('A', 'A'), ('A', 'B'), ('A', 'C'), ('A', 'D'), ('A', 'E'), ('B',
'A'), ('B', 'B'), ('B', 'C'), ('B', 'D'), ('B', 'E'), ('C', 'A'), ('C', 'B'), ('C', 'C'), ('C', 'D'), ('C', 'E'), ('D', 'A'), ('D', 'B'), ('D', 'C'), ('D', 'D'), ('D', 'E'), ('E', 'A'), ('E', 'B'), ('E', 'C'), ('E', 'D'), ('E', 'E')]
~~~

**그룹화**
주어진 값을 그룹화 시킬수도 있다.
~~~python
import itertools

gen9 = itertools.groupby('AAABBCCCCDDEE')

for chr, group in gen9:
    print('Ex6-12 - ', chr, ' : ', list(group))
~~~

~~~
Ex6-12 -  A  :  ['A', 'A', 'A']
Ex6-12 -  B  :  ['B', 'B']
Ex6-12 -  C  :  ['C', 'C', 'C', 'C']
Ex6-12 -  D  :  ['D', 'D']
Ex6-12 -  E  :  ['E', 'E']
~~~

&nbsp;
&nbsp;
&nbsp;

# 출저
1. [http://schoolofweb.net/blog/posts/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%A0%9C%EB%84%88%EB%A0%88%EC%9D%B4%ED%84%B0-generator/](http://schoolofweb.net/blog/posts/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%A0%9C%EB%84%88%EB%A0%88%EC%9D%B4%ED%84%B0-generator/)

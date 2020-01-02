---
layout: post
title: 재귀 함수 (Recursive Call)
tags:  [algorithm-and-data-structure]
---

> **고급 정렬 알고리즘에서 재귀 용법을 사용하므로, 고급 정렬 알고리즘을 익히기 전에 알아둬야 한다.**

재귀함수란 **함수 안에서 동일한 함수를 호출하는 형태이다.** 여러 알고리즘 작성시 사용되므로, 익숙해져야 한다. 또한 재귀 용법은 **일종의 패턴** 이 있으므로 잘 알아둬야 한다.

&nbsp;

### 1. 재귀 용법 이해

팩토리얼을 구하는 알고리즘을 recursive call을 활용하여 작성해보자.

먼저 간단하게 생각을 해보면,

2! = 2 x 1
3! = 3 x 2 x 1
4! = 4 x 3 x 2 x 1
이다.

혹은
2! = 2 x factorai(1)
3! = 3 x factorai(2)
4! = 4 x factorai(3)
이 된다.

즉,
**함수(n) 은 n > 1 이면 return n X 함수(n - 1)
함수(n) 은 n = 1 이면 return n**

**검증을 해보면**
* 먼저 2! 부터
함수(2) 이면, 2 > 1 이므로 2 X 함수(1)
함수(1) 은 1 이므로, return 2 X 1 = 2 맞다!

* 먼저 3! 부터
함수(3) 이면, 3 > 1 이므로 3 X 함수(2)
함수(2) 는 결국 1번에 의해 2! 이므로, return 2 X 1 = 2
3 X 함수(2) = 3 X 2 = 3 X 2 X 1 = 6 맞다!

* 먼저 4! 부터
함수(4) 이면, 4 > 1 이므로 4 X 함수(3)
함수(3) 은 결국 2번에 의해 3 X 2 X 1 = 6
4 X 함수(3) = 4 X 6 = 24 맞다!

~~~python
def factorial(num):
    if num > 1:
        return num * factorial(num - 1)
    else:
        return num

for index in range(10):
    print(factorial(index))
~~~
결과값은 아래와 같다.
~~~
0
1
2
6
24
120
720
5040
40320
362880
~~~

**시간복잡도 구하기**
factorial(n)은 n - 1번의 factorial 함수를 호출한 후, 곱하기 때문에 시간복잡도는 O(n)이다.

> **파이썬의 경우 재귀함수 호출을 1000번까지 할 수 있다. 그 이후로 하게 되면 error가 발생한다.**

~~~python
print(factorial(100000))
~~~
error는 아래와 같다.
~~~
RecursionError: maximum recursion depth exceeded in comparison
~~~

&nbsp;

### 2. 일반적인 재귀함수 형태

일반적으로 재귀함수 형태는 크게 **2가지** 가 같다. 첫번째와 두번째는 사실상 동일하지만 일정값보다 크냐 작으냐에 따라 차이가 있다.

**재귀함수 형태 # 1**
~~~
def recur_func(입력):
    if 입력 > 일정값 : #일정값보다 크다면
        return recur_funct(입력 - 1)
    else:
        return 일정값
~~~

**재귀함수 형태 2 # 2**

~~~
def recur_func(입력):
    if 입력 < 일정값:
        return 일정값

    recur_func(입력 - 1)

    return 결과값
~~~

&nbsp;

### 3. 재귀 호출은 스택의 전형적인 예¶
재귀함수는 내부적오르 스택처럼 관리된다.

![Alt text](/public/post/2020_01_02_recursive_call/recursivecall_stack.png)

&nbsp;

### 4. 프로그래밍 예

##### 1. 재귀 함수를 사용해서 1부터 num까지의 곱이 출력되게 만들어라.
~~~python
def multiple(num):

    if num > 1:
        return num * mul(num - 1)
    else:
        return num

def multiple2(num):

    if num <= 1:
        return num
    else:
        return num * multiple2(num - 1)

for num in range(10):
    print(multiple(num), multiple2(num))
~~~

결과값은
~~~
0 0
1 1
2 2
6 6
24 24
120 120
720 720
5040 5040
40320 40320
362880 362880
~~~

##### 2. 숫자가 들어 있는 리스트가 주어졌을 때, 리스트의 합을 리턴하는 함수를 만들어라.

~~~python
def sum_list(data_list):

    if len(data_list) > 1:
        return data_list.pop() + sum_list(data_list)
    else:
        return data_list.pop()

def sum_list(data):
    if len(data) <= 1 :
        return data[0]
    else:
        return data[0] + sum_list(data[1:])

print(sum_list([1,2,3]))
~~~


##### 3. Palindrome 문제
~~~python
def palindrome(word):    
    if len(word) > 1:
        if word[0] == word[-1]:
            return palindrome(word[1:-1])
        else:
            return False
    else:
        return True

def palindrome(word):
    if len(word) <= 1:
        return True
    else:
        if word[0] == word[-1]:
            return palindrome(word[1:-1])
        else:
            return False
~~~

##### 4. 프로그래밍 연습

1. 정수 n에 대해
2. n이 홀수이면 3 x n + 1을 하고
3. n이 짝수이면 n을 2로 나눈다.
4. 이렇게 계속 진행해서 n이 결국 1이 될때까지 2와 3의 과정을 반복한다.

예를 들어 n에 3을 넣으면,
3
10
5
16
8
4
2
1
이 된다.

이렇게 정수 n을 입력받아, 위의 알고리즘 처럼 1이 되는 과정을 모두 출력하는 함수를 작성하라.

~~~python
def cal(n):
    if n == 1:
        print("1")
    else:
        if n % 2 == 0:
            print(n)
            n = int(n / 2)
            return cal(n)
        else:
            print(n)
            n = n * 3 + 1
            return cal(n)

def func(n):
    print(n)
    if n == 1:
        return n

    if n % 2 == 1:
        return (func((3 * n) + 1))
    else:
        return (func(int(n / 2)))
~~~

##### 5.  프로그래밍 연습

정수 4를 1, 2, 3의 조합으로 나타내는 방법은 다음과 같이 총 7가지가 있다.
1+1+1+1
1+1+2
1+2+1
2+1+1
2+2
1+3
3+1
정수 n이 입력으로 주어졌을 때, n을 1, 2, 3의 합으로 나타낼 수 있는 방법의 수를 구하시오
힌트: 정수 n을 만들 수 있는 경우의 수를 리턴하는 함수를 f(n) 이라고 하면, f(n)은 f(n-1) + f(n-2) + f(n-3) 과 동일하다는 패턴 찾기

~~~python
def acp(n):
    if n == 1:
        return 1
    elif n == 2:
        return 2
    elif n == 3:
        return 4
    else:
        return acp(n-1) + acp(n-2) + acp(n-3)

print(acp(5))
~~~

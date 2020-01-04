---
layout: post
title: 동적계획법과 분할 정복 (Dynamic Programming and Divide and Conquer)
tags:  [algorithm-and-data-structure]
---

### 동적계획법 (DP라고 부름)

* **입력 크기가 작은 부분 문제들을 해결 한 후,** 해당 부분 문제의 **해를 활용해서 보다 큰 크기의 부분 문제를 해결,** 최종적으로 **전체 문제를 해결하는 알고리즘.**
* 상향식 접근법으로, 가장 최하위 해답을 구한 후, 이를 저장하고, 해당 결과값을 이용해서 상위 문제를 풀어가는 방식
* **Memoization 기법을 사용함**
    * Memoization (메모이제이션) 이란 : 프로그램 실행 시 **이전에 계산할 값을 저장하여, 다시 계산하지 않도록 하여 전체 실행 속도를 빠르게 하는 기술.**
* 문제를 잘게 쪼갤 때, 부분 문제는 중복되어 **재활용됨.**
    예 : 피보나치 수열

&nbsp;

### 분할 정복

* 문제를 **나눌 수 없을 때까지 나누어서 각각을 풀면서 다시 합병하여 문제의 답을 얻는 알고리즘.**

* **하양식 접근법**으로, 상위의 해답을 구하기 위해, 아래로 내려가면서 하위의 해답을 구하는 방식

    * 일반적으로 **재귀함수**로 구현

* **문제를 잘게 쪼갤 때, 부분 문제는 서로 중복되지 않는다.** ==> 동적계획법과 가장 큰 **차이점**
    * 예 : 병합 정렬, 퀵 정렬 등.

&nbsp;

### 공통점과 차이점

* 공통점
    * 문제를 잘게 쪼개서, 가장 작은 단위로 분할

* 차이점
    * 동적 계획법
        * 부분 문제는 중복되어, 상위 문제 해결 시 재활용됨.
        * Memoization 기법 사용 (부분 문제의 해답을 저장해서 재활용하는 최적화 기법으로 사용)
    * 분할 정복
        * 부분 문제는 서로 중복되지 않는다.
        * Memoization 기법 사용 안한다.

### 동적 계획법 알고리즘 이해

##### 1. 프로그래밍 연습 : 피보나치 수열

![Alt text](/public/post/2020_01_03_DP_DQ/Fibonacci.png)

![Alt text](/public/post/2020_01_03_DP_DQ/Fibonacci_2.png)

피보나치 수열을 2가지 방법으로 풀 수 있다. 첫번 째는 recursive call을 사용하는 방법과 두번 째는 동적 계획법을 사용하는 방법이 있다.

##### recursive call 활용
~~~python
def fibo(num):
    if num <= 1:
        return num
    else:
        return fibo(num - 1) + fibo(num - 2)
~~~

##### 동적 계획법 활용
~~~python
def fibo(num):
  cache = [0 for index in range(num + 1)]
  cache[0] = 0
  cache[1] = 1

  for index in range(2, num + 1):
    cache[index] = cache[index - 1] + cache[index - 2]

  return cache[num]
~~~

---
layout: post
title:  
tags:  [aws-docker]
---
코딩테스트에 나오는 수학은 정수론과 기하가 대표적이다.

코딩테스트 내의 기하는 피타고라스 정리를 활용한 점 사이의 거리등 쉽게 알 수 있는 내용이거나, CCW, 컨벡스헐, 좌표기하 등 내용이 어려워 난이도 격차가 꽤 있는 분야이다.

그래서 주로 아래와 같은 수학 주제가 코딩테스트에 나온다.

* gcd / lcm
* 소수 체크와 소인수 분해
* 에라토스테네스의 체 활용
* 거듭제곱 연산

# GCD와 LCM
__Greatest Common Divisor__ 와 __Least Common Multiple__ 은 가장 많이 나오는 유형 중 하나이다. 최대공약수 / 최소공배수를 묻는 문제의 90% 이상은 이 알고리즘을 사용한다.

__최소 공배수__ 의 경우에는 다음과 같은 식으로 풀 수 있으므로 __최대공약수__ 만 알면 된다.

![Alt text](/public/post/2020_03_22_Math/gcd_lcm_formula.png)

총 3개의 방법을 사용한다.

* 단순 반복문으로 하는 방법
* 유클리드 호제법을 이용한 방법
* 라이브러리를 사용하는 방법

**유클리드 호제법** 의 경우는 다음 성질을 활용한 방법이다.


![Alt text](/public/post/2020_03_22_Math/euclid.PNG)

~~~python
# 방법 1 : 단순하게 반복문 사용
def gcd_naive(a,b):
    for i in range(min(a,b), 0, -1):
        if a % i == 0 and b % i == 0: return i

# 방법 2 : 유클리드 호제법을 이용한 방법
def gcd(a,b):
    return gcd(b, a%b) if a % b != 0 else b

# 방법 2-2 : 반복문으로 변경
def gcd2(a,b):
    while a % b != 0 : a, b = b, a % b

    return b

# 방법 3 : math의 gcd 사용
import math
math.gcd(1,2)

%time print(gcd_naive(10**15, 2**25))
%time print(gcd(10**15, 2**25))
%time print(gcd2(10**15, 2**25))
%time print(math.gcd(10**15, 2**25))

# 라이브러리가 가장 빠르다. C, C++로 MAPPING이 되어 있기 떄문에 가장 빠르다.
~~~

~~~
32768
Wall time: 5.61 s
32768
Wall time: 0 ns
32768
Wall time: 0 ns
32768
Wall time: 0 ns
10,4
1
~~~

####  LCM은 gcd를 활용하여 계산하자
만약 python이 아닌 다른 언어의 경우 int overflow가 발생할 수 있으니 __a / gcd(a,b) * b 순서__ 로 하는 것을 추천한다.

~~~python
# LCM은 gcd를 활용하여 계산하자
# 만약 python이 아닌 다른 언어의 경우 int overflow가 발생할 수 있으니
# a / gcd(a,b) * b 순서로 하는 것을 추천한다.

def lcm(a,b):
    return a * b / gcd(a,b)
~~~

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;


# 소수 체크와 소인수 분해
소수 체크와 소인수 분해도 꽤 많이 나오는 유형이다. 소수 체크는 반복문으로 진행하면 되고, 소인수 분해의 경우는 조금의 트릭으로 진행하면 된다.

두 알고리즘 모두 시간복잡도는

![Alt text](/public/post/2020_03_22_Math/root_n.PNG)

이다.

~~~python
# 소수 체크 기본

def is_prime(N):
    for i in range(2, N):
        if N % i == 0:
            return False
        if i **2 >= N:
            break

    return True

# 소인수분해 기본
def prime_factorization(N):
    p, fac = 2, []

    while p ** 2 <= N:
        if N % p == 0:
            N //= p
            fac.append(p)
        else:
            p += 1

    if N > 1: fac.append(N)

    return fac
~~~

~~~python
print(prime_factorization(999999999))
print(prime_factorization(333667))
~~~

~~~
[3, 3, 3, 3, 37, 333667]
[333667]
~~~

### 에라토스테네스의 체
이런 알고리즘이 단 한 번 사용하거나 빠르게 체크할 때는 좋지만 여러 쿼리를 묻는 문제 등의 경우에는 시간복잡도가 꽤 크다.

이런 문제를 해결하기 위해 소수 리스트를 미리 만드는 방법이 있는데 이것이 __에라토스테네스의 체__ 이다.

예를 들어, 아래와 같이 있다면
2,3,4,5,6,7,8,9,10

2의 배수를 다 지워준다. ==> 2 3 5 7 9

3의 배수를 다 지워준다. ==> 2 3 5 7

이제 각 활용을 살펴보자.

1. 소인수의 개수

2. 소인수의 합

3. 소인수 분해를 위한 또 하나의 트릭

~~~python
# 활용 1 : 소인수의 개수
def era_factor_count(N):
    A = [0 for _ in range(N + 1)]

    for i in range(1, N):
        for j in range(i, N, i):
            A[j] += 1

    return A

# 활용 2 : 소인수의 합
def era_factor_sum(N):
    A = [0 for _ in range(N + 1)]
    for i in range(2, N):
        for j in range(i, N, i):
            A[j] += i

    return A

# 활용 3 : 소인수분해 하기
def era_factorization(N):
    A = [0 for _ in range(N + 1)]
    for i in range(2, N):

        if A[i]: continue

        for j in range(i, N, i):
            A[j] = i

    return A
~~~

&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 빠른 거듭제곱과 모듈러
python에서는 크게 많이 고민할 부분은 아니지만 거듭제곱 연산을 해야할 때가 많다. 이런 거듭제곱을 순수하게 반복문으로 진행하는 것이 아니라 효율적인 방법을 살펴보자.

__a^11 = a^1 * a^2 * a^8__


~~~python
# C/C++ Style
def pow_advanced(a, b, mod):
    ret = 1

    while b > 0:
        if b % 2 : ret = ret*a%mod
        a,b = a*a%mod, b // 2

    return ret

%time pow_advanced(3, 10000000, 1000000007)
%time pow(3, 10000000, 1000000007)
%time 3**10000000%1000000007
~~~

~~~
Wall time: 0 ns
Wall time: 0 ns
Wall time: 3.68 s
769346453
~~~

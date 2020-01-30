---
layout: post
title: 요약통계량
tags:  [Quant]
---
# 통계를 알아야 하는 이유?
금융에 수리적, 계량적 기법을 적용하는 사람들을 퀀트라고 한다. 컴퓨터를 이용해서 수치화하고 최적의 알고리즘을 적용하여 종목 및 전략을 세운다.

수치화 하고 최적의 알고리즘을 찾기 위해서는 기술 통계학 지식이 필요하다.

**기술통계학** : 자료를 요약 및 정리하여 전체 자료의 특성을 파악하고 쉽게 해석하기 위해서 사룡하는 통계기법이다.

**1. 평균**
**2. 중앙값**
**3. 분산**
**4. 표준편차**

# 산술평균
산술평균은 우리가 가장 흔히 보는 평균을 구하는 방법이다.

![Alt text](/public/post/2020_01_30_Quant_statistics/arti_mean.PNG)

~~~python
sec5 = [45500, 46550, 45850, 44050, 43900]

def mean(stock):
    return sum(stock) / len(stock)

mean(sec55)
~~~
~~~
45170.0
~~~

&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 기하평균

- **기하평균** 은 **n개의 양수 값을 모두 곱한 것의 n 제곱근** 이다.

- 곱셈으로 계산하는 값에서의 평균을 계산하고자 할 때, 산술 평균이 아닌 기하 평균을 사용한다.


예를 들어, 어떤 값이 처음에 1000이고 첫 해에 10%증가하고, 그 다음해에 20%증가하고 그 다음 해에 15% 감소했다고 하자.

그러면 수익률의 평균은 (10 + 20 - 15) / 3 = 5%가 아니라, (1.1 x 1.2 x 0.85) ^(1/3) = 1.0391... 이므로 3년동안 평균 3.91%씩 증가한 셈이다.

즉, 1000 X 1.1 X 1.2 X 0.85 = 1000 X (1.0391)^3이다.

~~~python
from math import *
def geometrical(*arg):
    print(arg)
    geometrical_list = [1 + (x/100) for x in arg]
    print(geometrical_list)
    total = 1
    for x in geometrical_list:
        total *= x
    return pow((total), 1/len(geometrical_list))

# 위의 예시에 사용되었던 10%, 20%, -15%를 인자로 넘겨주자.
print(geometrical(10, 20, -15)) # 위에서 구한 1.0391이 나온다.

1000 * geometrical(10, 20, -15) ** len([10, 20, -15])
~~~
~~~
(10, 20, -15)
[1.1, 1.2, 0.85]
1.0391166068457145
(10, 20, -15)
[1.1, 1.2, 0.85]
1122.0
~~~

&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 최댓값, 최솟값

**max()**, **min()** 함수는 내장함수로 각각 최댓값과 최솟값을 반환한다.

~~~python
print(max([10,20,30,40]))
print(min([10, 20, 30, 40]))
~~~

# 중앙값

중앙값이란 데이터를 순서대로 늘어놓았을 때 그 중간에 오는 값이다.

데이터 개수가 홀수인 경우 가장 중앙에 있는 데이터이다.
데이터 개수가 짝수인 경우 중앙에 있는 두 값의 평균이다.

~~~python
test_1 = [i for i in range(8)]
test_2 = [i for i in range(7)]

def median(data):

    data.sort()

    if len(data) % 2 != 0:
        middle = int(len(data) / 2)
        return data[middle]
    else:
        left = int(len(data) / 2) - 1
        right = int(len(data) / 2)

        return (data[left] + data[right]) / 2

print(test_1)
print(median(test_1))
print(test_2)
print(median(test_2))
~~~
~~~
[0, 1, 2, 3, 4, 5, 6, 7]
3.5
[0, 1, 2, 3, 4, 5, 6]
3
~~~

# 분산과 표준편차

**분산은 평균값 주변에 데이터들이 모여있는 지 흩어져 있는 지 알려주는 데이터** 이다.

분산의 수식은 평균값의 차이의 제곱들의 합이다.

![Alt text](/public/post/2020_01_30_Quant_statistics/variance.PNG)

**편차** 는 데이터의 값과 평균값의 차이를 의미한다. **표준편차** 는 **분포된 정도를 표시하며 데이터가 평균에서 얼마나 떨어져 있는 지 알려주는 지표** 이다.

![Alt text](/public/post/2020_01_30_Quant_statistics/variance_2.PNG)

~~~python
def variance(data):
    vari_mean = mean(data)
    diff = 0
    for n in data:
        diff += (n - vari_mean) ** 2
    return diff / (len(data) - 1) #표본의 표준편차를 구할 때는 -1을 적용한다.
~~~

~~~python
def stddev(data):
  return variance(data) ** 0.5
~~~

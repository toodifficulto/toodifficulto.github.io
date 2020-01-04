---
layout: post
title: 퀵 정렬 (Quick Sort) [고급정렬]
tags:  [algorithm-and-data-structure]
---

**정렬 알고리즘의 꽃이다.**
‘찰스 앤터니 리처드 호어(Charles Antony Richard Hoare)’가 개발한 정렬 알고리즘이다. 퀵 정렬은 불안정 정렬 에 속하며, 다른 원소와의 비교만으로 정렬을 수행하는 비교 정렬 에 속한다.

**분할 정복** 알고리즘의 하나로, 평균적으로 매우 빠른 수행 속도를 자랑하는 정렬 방법이다. 합병 정렬(merge sort)과 달리 퀵 정렬은 리스트를 비균등하게 분할한다.

![Alt text](/public/post/2020_01_03_Quick_Sort/animation.gif)

&nbsp;

### 과정설명

기준점(pivot)을 정해서, 기준점보다 작은 데이터는 왼쪽(left), 큰 데이터는 오른쪽(right)으로 모으는 함수를 작성한다.

각 왼쪽(left), 오른쪽(right)은 재귀용법을 사용해서 다시 동일 함수를 호출하여 위 작업을 반복한다.

함수는 왼쪽(left) + 기준점(pivot) + 오른쪽 (right)을 리턴한다.

1. 리스트 안에 피벗을 선택한다. (주로 제일 첫번째 있는 원소)
2. 피벗을 기준으로 피벗보다 작은 원소는 왼쪽에, 큰 원소는 오른쪽에 있는 리스트에 넣는다.
3. 피봇을 제외한 왼쪽 리스트와 오른쪽 리스트를 다시 정렬한다.
  * 순환호출을 사용하여 재정렬을 한다.
4. 부분 리스트들이 더 이상 분할이 불가능할때까지 진행한다.

![Alt text](/public/post/2020_01_03_Quick_Sort/quick_sort_ex.png)

&nbsp;

### 파이썬으로 구현하기.

~~~python
def qsort(data):
    if len(data) <= 1:
        return data

    left, right = list(), list()
    pivot = data[0]

    for index in range(1, len(data)):
        if pivot > data[index]:
            left.append(data[index])
        else:
            right.append(data[index])

    return qsort(left) + [pivot] + qsort(right)
~~~
테스트를 해보면,

~~~python
import random

data_list = random.sample(range(100), 10)
qsort(data_list)
~~~
아래와 같은 결과가 나온다.

~~~
[1, 13, 22, 27, 35, 58, 64, 74, 92, 99]
~~~

&nbsp;

### 알고리즘 분석

병합정렬과 유사, 시간복잡도는 **O(nlog n)** 이다. 단, 최악의 경우 맨 처음 pivot이 가장 크거나, 가장 작으면 아래의 그림처럼 모든 데이터를 비교하는 상황이 나온다. ==> **O(n2)**

![Alt text](/public/post/2020_01_03_Quick_Sort/quicksortworks.jpg)

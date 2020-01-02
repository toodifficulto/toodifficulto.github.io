---
layout: post
title: [기본 정렬 알고리즘]선택 정렬 (Selection Sort)
tags:  [algorithm-and-data-structure]
---

**선택 정렬** 은 알고리즘 중에서도 버블 정렬과 함께 가장 간단한 알고리즘에 속한다.

* 다음과 같은 순서를 반복하여 정렬하는 알고리즘
    1. 주어진 데이터 중, **최소값** 을 찾는다.
    2. 해당 최소값을 **데이터 맨 앞에 위치한 값과 교체한다.**
    3. 맨 앞 의 위치를 뺀 나머지 데이터를 **동일한 방법으로 반복** 한다.    

![Alt text](/public/post/2020_01_02_selection_sort/Selection-Sort-Animation.gif)

&nbsp;

### 어떻게 코드를 짤까?

데이터가 **2개 일때, 3개 일때, 4개 일때를 생각하면서 간단하게 작성을 해보자.**

* 데이터가 두 개 일때
예: dataList = [9, 1]
data_list[0] > data_list[1] 이므로 datalist[0] 값과 data list[1] 값을 교환

* 데이터가 세 개 일때
예: data_list = [9, 1, 7]
처음 한번 실행하면, 1, 9, 7 이 됨
두 번째 실행하면, 1, 7, 9 가 됨

* 데이터가 네 개 일때
예: data_list = [9, 3, 2, 1]
처음 한번 실행하면, 1, 3, 2, 9 가 됨
두 번째 실행하면, 1, 2, 3, 9 가 됨
세 번째 실행하면, 변화 없음

**data_list의 길이보다 1 작은 횟수** 만큼 돌아간다. 그리고 **비교가 시작되는 index값은 1씩 늘어난다.**

**pseudocode 코드를 작성해보자.**

~~~
for index in range(len(data_list) - 1):

    smallest_index = index

    for index2 in range(index + 1, len(data_list)):
        if data_list[smallest_index] < data_list[index2]:
            smallest_index = index2

    swap(data_list[smallest_index], data_list[index]) # 맨 앞에 있는 값과 바꿔준다.
~~~

&nbsp;

### 실제 코드를 작성해보자.

~~~python
def selection_sort(data_list):

    for index in range(len(data_list) - 1):

        smallest_index = index # 가장 작은 숫자의 index

        for index2 in range(index + 1, len(data_list)):
            if data_list[smallest_index] > data_list[index2]: # 만약 더 작은 숫자가 있다면
                smallest_index = index2 # index를 바꾼다.

        data_list[smallest_index], data_list[index] = data_list[index], data_list[smallest_index]

    return data_list
~~~

그리고 테스트를 해보자.
~~~python
import random

for i in range(10):
    data_list = random.sample(range(100), 10)
    print(selection_sort(data_list))
~~~
결과값은 아래와 같다.
~~~
[4, 5, 8, 24, 34, 36, 61, 69, 83, 91]
[6, 31, 36, 50, 68, 71, 72, 88, 90, 99]
[1, 6, 10, 53, 56, 57, 75, 89, 91, 96]
[28, 33, 37, 50, 52, 60, 73, 80, 86, 89]
[4, 24, 25, 28, 52, 54, 66, 73, 78, 81]
[6, 9, 15, 52, 64, 69, 75, 85, 88, 93]
[2, 6, 16, 24, 28, 35, 36, 67, 72, 84]
[1, 32, 33, 40, 41, 43, 44, 62, 75, 95]
[20, 26, 40, 49, 51, 75, 76, 86, 90, 96]
[4, 5, 15, 28, 32, 48, 53, 56, 68, 82]
~~~

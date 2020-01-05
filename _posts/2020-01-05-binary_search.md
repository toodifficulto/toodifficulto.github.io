---
layout: post
title: 이진 탐색 (Binary Search) [탐색]
tags:  [algorithm-and-data-structure]
---
탐색 알고리즘은 저번 자료구조 시간에 이진 탐색 트리, 해쉬 트리, 순차 탐색등으로 살펴본 적이 있다. 이번에는 이진 탐색이라는 또 다른 방법으로 탐색에 대해 알아보도록 하자.

이진 탐색이란 **탐색할 자료를 둘로 나누어 해당 데이터가 있을만한 곳을 탐색하는 방법**이다. 단, 이진 탐색은 **데이터가 정렬되어 있는 상태에서 진행** 되어야 한다.

아래 에니메이션을 보면 **데이터가 정렬되어 있는 상태에서** 이진 탐색은 먼저 탐색할 자료를 둘로 나누고 중간의 값보다 크면 오른쪽을 다시 반으로, 작으면 왼쪽을 다시 반으로 계속 나눈다. 반면, 순차 탐색은 첫 원소부터 하나씩 찾는다.

![Alt text](/public/post/2020_01_05_binarysearch/binarysearch_ani.gif)

&nbsp;

### 1. 분할 정복 알고리즘과 이진 탐색

* 분할 정복 알고리즘 (Divide and Conquer)
    * Divide : 문제를 하나 또는 둘 이상으로 나눈다.
    * Conquer : 나눠진 문제가 충분히 작고, 해결이 가능하다면 해결하고 그렇지 않다면 다시 나눈다.
    * 재귀용법을 사용한다.

* 이진 탐색
    * Divide : 리스트를 두 개의 서브 리스트로 나눈다
    * Conquer
        * 검색할 숫자 (search) > 중간값이면, 뒷 부분의 서브 리스트에서 검색할 숫자를 찾는다.
        * 검색할 숫자 (search) < 중간값이면, 앞 부분의 서브 리스트에서 검색할 숫자를 찾는다.

&nbsp;

### 2. 어떻게 코드를 만들까?

* 이진 탐색은 **데이터가 정렬되어 있는 상태에서 진행한다.**
* 데이터가 [2,3,8,12,20] 일 때,
    * binary_search(data_list, find_data) 함수를 만들고
        * find_data는 찾는 숫자
        * data_list는 데이터 리스트
        * data_list의 중간값을 find_data와 비교해서
                * find_data < data_list의 중간값이라면,
                    * 맨 앞부터 data_list의 중간까지 에서 다시 찾기
                * find_data > data_list의 중간값이라면,
                    * 맨 뒤부터 data_list의 중간까지 에서 다시 찾기
                * 그렇지 않다면 return True

&nbsp;

### 3. 알고리즘 구현

아래는 내가 작성한 코드이다.
~~~python
def binary_search(data_list, find_data):

    if len(data_list) == 1 and data_list[0] != find_data:
        return False

    half = int(len(data_list) / 2)

    if data_list[half] == find_data:
        return True
    else:
        if data_list[half] > find_data:
            return binary_search(data_list[:half], find_data)
        else:
            return binary_search(data_list[half:], find_data)
~~~

동영상에서 본 코드.
~~~python
def binary_search(data, search):
    if len(data) == 1 and search == data[0]: # 마지막으로 남은 하나의 값이 원하는 값이면
        return True

    if len(data) == 1 and search != data[0]: # 마지막으로 남은 하나의 값이 원하는 값이 아니라면
        return False

    if len(data) == 0: # 가능성은 없지만 방어 코드
        return False

    medium = len(data) // 2

    if search == data[medium]:
        return True
    else:
        if search > data[medium]:
            return binary_search(data[medium:], search)
        else:
            return binary_search(data[:medium], search)
~~~

테스트를 해보자.
~~~python
import random

data_list = random.sample(range(100), 10)
data_list

# 이진 탐색의 조건은 정렬되어 있어야 한다.
data_list.sort() # data_list가 sorting 된다.

binary_search(data_list, 22)
~~~

&nbsp;

### 4. 알고리즘 분석

n개의 리스트를 매번 2로 나누어서 1이 될때까지 비교연산을 k번 진행한다.

![Alt text](/public/post/2020_01_05_binarysearch/timecomplexity.PNG)

빅 오 표기법으로 k + 1이 결국 최종 시간 복잡도이다. 즉, O(log2n + 1)이고 2와 1은 삭제되므로 **O(logn)** 이 된다.

---
layout: post
title: 병합 정렬 (Quick Sort) [고급정렬]
tags:  [algorithm-and-data-structure]
---
**병합 정렬** 은 Divide and conquer 분할정복을 사용하는 대표적인 알고리즘이다. 분할정복 알고리즘 답게 문제를 잘게 쪼개서 푼다. Memoization등을 사용하는 동적 계획법과 다르다.

1. 먼저 리스트를 비슷한 크기의 두 개의 리스트로 나눈다.
2. 두 개로 나뉘어진 리스트를 계속해서 각각 두 개의 리스트로 한개 씩 원소로 나뉘어질 때까지 계속 나눈다.
3. 각각 하나씩 나뉘어진 원소를 2개로 병합시키는 병합 정렬을 재귀적으로 호출하면서 정렬을 한다.
4. 두 부분 리스트를 다시 하나의 리스트로 병합한다.

아래 애니메이션을 보면 더 확실히 이해하기가 쉽다.

![Alt text](/public/post/2020_01_04_Merge_Sort/mergesort_animation.gif)

&nbsp;

### 2. 알고리즘의 이해

##### 데이터가 4개 있다고 가정을 해보자. [1,9,3,2]
data_list =[1,9,3,2]

1. [1,9]와 [3,2]로 나누고
2. 다시 [1],[9]로 나누고 [3],[2]로 나눈다.
3. [1],[9]를 정렬해서 [1,9]로 합병한다.
4. [3],[2]를 정렬해서 [2,3]로 합병한다.
5. 이제 [1,9]와 [2,3]을 다시 합친다.
    * 먼저 첫번째 원소끼리 비교한다. 1 < 2 니까 [1]
    * 다음 원소와 2를 비교한다. 9 > 2 니까 [1,2]
    * 마지막 원소끼리 비교한다. 9 >3 니까 [1,2,3]
    * 9밖에 없으니까 [1,2,3,9]


&nbsp;

### 3. 알고리즘 짜기
데이터를 나누는 **Split함수** 와 데이터를 다시 합병하는 **merge함수** 2가지가 있어야 한다. pseduocode를 한번 짜보자.

~~~
def split(data_list):

    if len(data_list) == 1: #만약 data_list의 길이가 1이면 list를 반환한다.
        return list
    # 아니라면, 2개로 쪼개야 한다.
    else:
        left_list = data_list[:half] # 0부터 반까지의 리스트
        right_list = data_list[haf:] # 반부터 끝까지의 리스트

        return merge(split(left_list), split(right_list))

def merge(left, right):

    # left와 right을 비교한다.
    if left[lp] < right[rp]: #만약 왼쪽 list의 left pointer가 가리키는 원소가 right pointer가 가리키는 원소보다 작으면
        list.append(left[lp]) # 원쪽 원소를 넣는다.
        lp++ # 왼쪽 pointer를 옮긴다.
    else:
        list.append(right[rp]) # 반대로 진행한다.
        rp++

    return list
~~~

&nbsp;

### 4. 파이썬으로 구현하기

~~~python
def split(data_list):

    if len(data_list) == 1:
        return data_list
    else:
        half = int(len(data_list)/2)

        left = split(data_list[:half])
        right = split(data_list[half:])

        return merge(left, right)

def merge(left, right):

    merged = list()

    left_point, right_point = 0,0

    # case1: left/right이 아직 남아있을 때
    while len(left) > left_point and len(right) > right_point:
        if left[left_point] > right[right_point]:
            merged.append(right[right_point])
            right_point += 1
        else:
            merged.append(left[left_point])
            left_point += 1

    # case2 : left만 남아있을 때
    while len(left) > left_point:
        merged.append(left[left_point])
        left_point += 1

    # case3 : right만 남아있을 때
    while len(right) > right_point:
        merged.append(right[right_point])
        right_point += 1

    return merged
~~~

~~~python
data_list = [9,8,7,6,5,4,3,2,1]

split(data_list)
~~~
실행해보면, 아래의 결과가 나온다.

~~~
[1, 2, 3, 4, 5, 6, 7, 8, 9]
~~~

&nbsp;

### 5. 알고리즘 분석
몇 단계 깊이까지 만들어지는지를 depth 라고 하고 i로 놓자. 맨 위 단계는 0으로 놓자.

다음 그림에서 n/2^2는 2단계 깊이라고 해보자.

각 단계에 있는 하나의 노드 안의 리스트 길이는 n/2^2 가 된다.

각 단계에는 2^i 개의 노드가 있다.

따라서, 각 단계는 항상 2^i∗n/2^i=O(n)

단계는 항상 log_2n 개 만큼 만들어짐, 시간 복잡도는 결국 O(log n), 2는 역시 상수이므로 삭제

따라서, 단계별 시간 복잡도 O(n) * O(log n) = **O(n log n)**

![Alt text](/public/post/2020_01_04_Merge_Sort/mergesortcomplexity.png)

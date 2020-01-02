---
layout: post
title: [기본 정렬 알고리즘]삽입 정렬 (Insertion Sort)
tags:  [algorithm-and-data-structure]
---

**삽입정렬** 은 선택정렬, 버블정렬과 같이 정렬 알고리즘에서 가장 간단한 알고리즘에 속한다.

1. 삽입 정렬은 두 번째 인덱스부터 시작
2. **해당 인덱스(key 값) 앞에 있는 데이터(B)부터 비교해서 key 값이 더 작으면, B값을 뒤 인덱스로 복사**
3. 이를 **key 값이 더 큰 데이터를 만날때까지 반복**, 그리고 **큰 데이터를 만난 위치 바로 뒤에 key 값을 이동**

글로 보면 헷갈릴 수 있으므로 아래 애니메이션을 보자.

![Alt text](/public/post/2020_01_02_insertion_sort/Insertion_sort_example.gif)

먼저 2번째 인덱스부터 시작을 한다. 그리고 해당 인덱스보다 앞에 있는 인덱스와 비교를 해서 해당 인덱스의 값보다 큰 값을 가지면 서로의 위치를 바꾼다. 그리고 다시 해당 인덱스보다 앞에 있는 인덱스와 계속 비교를 해나간다. 만약 해당 인덱스 값보다 더 작은 값을 가지고 있
다면 Swap을 그만둔다.

&nbsp;

### 어떻게 코드를 짤까?
데이터가 2개 일때, 3개 일때, 4개 일때를 생각하면서 간단하게 작성을 해보자.

데이터가 두 개 일때 예: dataList = [9, 1]
    - 첫번째 turn에서는 첫번째 index인 1에서부터 시작.
    - 1과 9를 비교. 1이 더 작으므로 swap ==> [1,9]

데이터가 세 개 일때 예: data_list = [9, 1, 7]
    - 첫번째 turn에서는 첫번째 index인 1에서부터 시작.
    - 1과 9를 비교 1이 더 작으으므로 swap ==> [1,9,7] trun이 끝난다.
    - 이제 2번째 turn시작. index는 2부터 시작한다. 7과 9를 비교한다. 7이 더 작으므로 swap ==> [1,7,9]. 이제 7과 1을 비교한다. 7이 더 크므로 swap하지 않는다.

데이터가 네 개 일때 예 : data_list = [9, 3, 2, 1]
    - 첫번 째 trun에서는 첫번째 index인 3에서 부터 시작.
    - 3과 9를 비교한다. 3이 더 작으므로 swap 한다. ==> [3,9,2,1]
    - 두 번째 turn에서는 두번 째 index인 2에서부터 시작한다. 2와 9를 비교한다. 2가 더 작으므로 swap한다. ==> [3,2,9,1] 그리고 2와 3을 비교한다. 2가 더 작으므로 swap한다. ==> [2,3,9,1]
    - 세 번째 turn에서는 세번 째 index인 1에서부터 시작한다. 1과 9를 비교한다. 1이 더 작으므로 swap한다. ==> [2,3,1,9] 1과 3을 비교한다. 더 작으므로 swap한다. ==> [2,1,3,9] 1과 2를 비교한다. 더 작으므로 swap한다. ==> [1,2,3,9]

data_list의 길이보다 **1 작은 횟수 만큼 돌아간다.** 그리고 **비교를 시작하는 index값은 1에서 시작하여 점점 증가한다.**

pseudocode 코드를 작성해보자.
~~~
def insertion_sort(data_list):

    for index in range(len(data_list) - 1):
        for index2 in range(index + 1, 1, -1):
            if data{index2] < data[index2 - 1]:
                swap(data[index2], data[index2 - 1])

            else:
                break
~~~

&nbsp;

### 실제 코드를 작성해보자
~~~python
def insertion_sort(data_list):

    for index in range(len(data_list) - 1):
        for index2 in range(index + 1, 0, -1):
            if data_list[index2] < data_list[index2 - 1]:
                data_list[index2], data_list[index2 - 1] = data_list[index2 - 1], data_list[index2]
            else:
                break

    return data_list
~~~
그리고 테스트를 해보자.
~~~python
import random

for i in range(10):
    data_list = random.sample(range(100), 10)
    print(insertion_sort(data_list))
~~~

결과값은 아래와 같다.
~~~
[1, 29, 31, 33, 37, 60, 62, 86, 91, 93]
[11, 12, 21, 37, 39, 40, 50, 71, 79, 88]
[9, 23, 24, 35, 65, 71, 73, 81, 82, 90]
[20, 27, 31, 32, 50, 54, 62, 74, 85, 87]
[6, 25, 44, 57, 58, 69, 85, 97, 98, 99]
[3, 30, 40, 55, 69, 70, 71, 72, 82, 99]
[1, 2, 3, 9, 17, 21, 25, 34, 35, 94]
[2, 26, 33, 39, 45, 58, 61, 64, 67, 95]
[11, 13, 15, 18, 29, 52, 75, 77, 88, 96]
[13, 37, 48, 52, 54, 58, 69, 75, 87, 93]
~~~

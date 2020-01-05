---
layout: post
title: 순차 탐색 (Sequential Search) [탐색]
tags:  [algorithm-and-data-structure]
---

탐색은 **여러 데이터 중에서 원하는 데이터를 찾아내는 것을 의미** 한다. 순차 탐색은 탐색 알고리즘 중에서 가장 쉽고 기본이 되는 알고리즘이다. 데이터가 담겨있는 리스트를 앞에서부터 **하나씩 비교해서 원하는 데이터를 찾는 방법** 이다.

~~~python
from random import *

read_data_list = list()

for num in range(10):
    read_data_list.append(randint(1, 100))

read_data_list
~~~

~~~
[19, 97, 19, 81, 94, 51, 72, 68, 5, 60]
~~~

~~~python
def seq_search(read_data_list, search):

    for index in range(len(read_data_list)):
        if read_data_list[index] == search:
            return index

    return False

seq_search(read_data_list, 19)

~~~
결과는 아래와 같이 잘 나온다.
~~~
0
~~~

&nbsp;

### 알고리즘 분석
최악의 경우 리스트의 길이가 n일때, n번 비교하기 때문에 **O(n)** 이다.

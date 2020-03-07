---
layout: post
title: Minimum Spanning Tree (1) Prim's Algorithm
tags:  [algorithm-and-data-structure]
---

# 1.  프림 알고리즘 (Prim's Algorithm)
프림 알고리즘은 크루스칼(Kruskal)알고리즘과 함께 대표적인 최소 신장 트리 알고리즘이다.

**대표적인 최소 신장 트리 알고리즘**
* Kruskal's Algorithm (크루스칼 알고리즘)
* Prim's algorithm(프림 알고리즘)

시작 정점을 선택한 후, 정점에 인접한 간선중 최소 간선으로 연결된 정점을 선택하고, 해당 정점에서 다시 최소 간선으로 연결된 정점을 선택하는 방식으로 최소 신장 트리를 확장해가는 방식이다.

##### Kruskal's algorithm 과 Prim's algorithm 비교
* 둘다, 탐욕 알고리즘을 기초로 하고 있다. (당장 눈 앞의 최소 비용을 선택해서, 결과적으로 최적의 솔루션을 찾음)


* Kruskal's algorithm은 가장 가중치가 작은 간선부터 선택하면서 MST를 구한다.


* Prim's algorithm은 특정 정점에서 시작, 해당 정점에 연결된 가장 가중치가 작은 간선을 선택, 간선으로 연결된 정점들에 연결된 간선 중에서 가장 가중치가 작은 간선을 택하는 방식으로 MST를 구한다.

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;


# 2. 알고리즘 순서

**1. 임의의 정점을 선택하고, '연결된 노드 집합'에 삽입한다.**

아래 그림을 보면 **A를** 선택하고 A를 **연결된 노드 집합에** 삽입한다.

**2. 선택된 정점에 연결된 간선들을 간선 리스트에 삽입한다.**

아래 그림에서 A와 연결된 간선(A,B)와 (A,D)를 **간선 리스트** 에 넣는다.

**3. 간선 리스트에서 최소 가중치를 가지는 간선부터 추출해서, 해당 간선에 연결된 인접 정점이 '연결된 노드 집합'에 이미 들어 있다면, 스킵한다.(cycle 발생을 막기 위함)**

아래 그림에 보면 (A,D)와 (A,B) 중 최소 가중치인 5를 가지는 (A,B)를 선택한다. 만약, B가 **연결된 노드 집합** 에 있다면 스킵한다. (하지만 속해 있지 않다.)

**3. 해당 간선에 연결된 인접 정점이 '연결된 노드 집합'에 들어 있지 않으면, 해당 간선을 선택하고, 해당 간선 정보를 '최소 신장 트리'에 삽입한다.**

아래 그림에 보면 (A,D)와 (A,D) 중 최소 가중치인 5를 가지는 (A,D)를 선택한다.D는 **연결된 노드 집합** 에 속해 있지 않으므로 (A,D)를 선택해서 '최소신장트리'에 넣는다.

**4. 추출한 간선은 간선 리스트에서 제거한다.**

(A,D)는 간선 리스트에서 뺀다.

**5. 간선 리스트에 더 이상의 간선이 없을 때까지 3-4번을 반복한다.**

그런 다음 이제 D를 기준으로 다시 시작한다. D와 연결된 (D,F), (D,E), (D,B)를 **간선 리스트** 에 추가하고 반복한다.

![Alt text](/public/post/2020_03_05_spanningtree_2/pic1.PNG)

![Alt text](/public/post/2020_03_05_spanningtree_2/pic2.PNG)

![Alt text](/public/post/2020_03_05_spanningtree_2/pic3.PNG)

# 3. 파이썬 코드
~~~python
myedges = [
    (7,'A', 'B'),(5,'A', 'D'),
    (8, 'B', 'C'), (9,'B', 'D'),(7,'B','E'),
    (5, 'C', 'E'),
    (7, 'D', 'E'), (6, 'D', 'F'),
    (8, 'E', 'F'), (9, 'E', 'G'),
    (11, 'F', 'G')
]

from collections import defaultdict
from heapq import *

def prim(start_node, edges):
    mst = list()
    adjacent_edges = defaultdict(list)
    for weight, n1, n2 in edges:
        adjacent_edges[n1].append((weight, n1, n2))
        adjacent_edges[n2].append((weight, n2, n1))

    # 연결된 노드들을 집합 형태로 넣는다.
    connected_nodes = set(start_node)
    candidate_edge_list = adjacent_edges[start_node]
    heapify(candidate_edge_list)

    while cand
    idate_edge_list:
        weight, n1, n2 = heappop(candidate_edge_list)
        if n2 not in connected_nodes:
            connected_nodes.add(n2)
            mst.append((weight, n1, n2))

            for edge in adjacent_edges[n2]:
                if edge[2] not in connected_nodes:
                    heappush(candidate_edge_list, edge)

    return mst

prim('A', myedges)
~~~

~~~
[(5, 'A', 'D'),
 (6, 'D', 'F'),
 (7, 'A', 'B'),
 (7, 'B', 'E'),
 (5, 'E', 'C'),
 (9, 'E', 'G')]
~~~

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;


# 3. 시간 복잡도
최악의 경우, while구문에서 모든 간선에 대해 반복하고 최소 힙 구조를 사용하므로 O(ElogE)의 시간복잡도를 가진다.

**하지만 주로 사용되는 프림 알고리즘은 O(ElogE) 대신 O(ElogV)의 시간 복잡도를 가진다. 즉 위의 알고리즘보다 향상된 성능의 알고리즘을 사용한다!**

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;


# 4. 개선된 프림 알고리즘

**간선이 아닌 노드를 중심으로 우선순위 큐를 적용하는 방식**

**초기화**

**1. 정점:key 구조를 만들어놓고, 특정 정점의 key값은 0, 이외의 정점들의 key값은 무한대로 놓는다.**

예를 들어 아래와 같은 그래프가 있다고 하자.

![Alt text](/public/post/2020_02_05_spanningtree_2/pic4.PNG)

그러면 이 그래프를 다음과 같이 초기화 한다. 특정 정점 A는 0, 나머지는 무한대로 놓는다.

![Alt text](/public/post/2020_03_05_spanningtree_2/pic5.PNG)

**2. 모든 정점:key 값은 우선순위 큐에 넣는다.**

그리고 A,B,C,D,E,F,G를 우선순위 큐에 넣는다.

**3. 가장 key값이 적은 정점: key를 추출한 후
(pop 하므로 해당 정점:key 정보는 우선순위 큐에서 삭제됨), (extract min 로직이라고 부름)**

그리고 가장 작은 값인 A (나머지는 무한대이므로)를 POP하여 우선순위 큐에서 삭제한다.

**4. 해당 정점의 인접한 정점들에 대해 key 값과 연결된 가중치 값을 비교하여 key값이 작으면 해당 정점:key 값을 갱신한다.**

A와 인접한 정점들 B,C에 대해서 각각 연결된 가중치 값 7, 5를 무한대와 비교해서 가중치 값이 작으면 정점의 값을 가중치 값(7,5)으로 갱신한다.

![Alt text](/public/post/2020_03_05_spanningtree_2/pic6.PNG)


**5. 정점:key 값 갱신시, 우선순위 큐는 최소 key값을 가지는 정점:key 를 루트노드로 올려놓도록 재구성한다. (decrease key 로직이라고 부름)**

그런 다음 (B,C,D,E,F,G)에서 가장 작은 키 값을 가지는 B를 (나머지는 7, ~,~,~,~,~ 이므로) 루트노드로 설정하고 다시 1부터 반복한다.

![Alt text](/public/post/2020_03_05_spanningtree_2/pic7.PNG)

&nbsp;
&nbsp;
&nbsp;
&nbsp;

### 파이썬 코드
~~~python
mygraph = {
    'A': {'B': 7, 'D': 5},
    'B': {'A': 7, 'D': 9, 'C': 8, 'E': 7},
    'C': {'B': 8, 'E': 5},
    'D': {'A': 5, 'B': 9, 'E': 7, 'F': 6},
    'E': {'B': 7, 'C': 5, 'D': 7, 'F': 8, 'G': 9},
    'F': {'D': 6, 'E': 8, 'G': 11},
    'G': {'E': 9, 'F': 11}    
}

from heapdict import heapdict

def improved_prim(graph, start):
    mst, keys, pi, total_weight = list(), heapdict(), dict(), 0

    for node in graph.keys():
        keys[node] = float('inf')
        pi[node] = None

    keys[start], pi[start] = 0, start

    while keys:
        current_node, current_key = keys.popitem()
        mst.append([pi[current_node], current_node, current_key])
        total_weight += current_key
        for adjacent, weight in mygraph[current_node].items():
            if adjacent in keys and weight < keys[adjacent]:
                keys[adjacent] = weight
                pi[adjacent] = current_node

    return mst, total_weight

improved_prim(mygraph, 'A')
~~~

~~~
([['A', 'A', 0],
  ['A', 'D', 5],
  ['D', 'F', 6],
  ['A', 'B', 7],
  ['D', 'E', 7],
  ['E', 'C', 5],
  ['E', 'G', 9]],
 39)
~~~

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;


# 5. 개선된 프림 알고리즘의 시간 복잡도

1. 최초 key 생성 시간 복잡도:  **𝑂(𝑉)**

2. while 구문과 keys.popitem() 의 시간 복잡도는  **𝑂(𝑉𝑙𝑜𝑔𝑉)**


3. while 구문은 V(노드 갯수) 번 실행된다. heap 에서 최소 key 값을 가지는 노드 정보 추출 시(pop)의 시간 복잡도:  𝑂(𝑙𝑜𝑔𝑉)
for 구문의 총 시간 복잡도는  **𝑂(𝐸𝑙𝑜𝑔𝑉)**

for 구문은 while 구문 반복시에 결과적으로 총 최대 간선의 수 E만큼 실행 가능  𝑂(𝐸)

따라서 총 시간 복잡도는 𝑂(𝑉+𝑉𝑙𝑜𝑔𝑉+𝐸𝑙𝑜𝑔𝑉) 이며, O(V)는 전체 시간 복잡도에 큰 영향을 미치지 않으므로 삭제, E > V 이므로 (최대 𝑉2=𝐸 가 될 수 있음),

𝑂((𝑉+𝐸)𝑙𝑜𝑔𝑉) 는 간단하게 **𝑂(𝐸𝑙𝑜𝑔𝑉)** 로 나타낼 수 있다.

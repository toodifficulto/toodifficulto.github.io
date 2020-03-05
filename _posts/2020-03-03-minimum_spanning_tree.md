---
layout: post
title: Minimum Spanning Tree (1) Kruskal's Algorithm
tags:  [algorithm-and-data-structure]
---
# 1. 신장 트리란?
Spanning Tree 또는 신장트리라고 부른다.

아래 그림을 보자. 원래 그래프는 왼쪽과 같다. 모든 노드가 연결되어 있으면서 트리의 속성을 만족하는 형태이다.

**신장 트리의 조건**
  * 본래의 그래프의 모든 노드를 포함해야 함
  * 모든 노드가 서로 연결
  * 트리의 속성을 만족시킨다. (사이클이 존재하지 않는다)

그리고 이 원래 그래프에서 위 신장트리의 조건을 만족하면 신장트리가 된다. 오른쪽은 원래 그래프에서 신장트리의 조건을 만족하는 그래프들의 집합이다.

![Alt text](/public/post/2020_03_03_spanningtree/pic1.PNG)

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;


# 2. 최소 신장 트리
* ***Minium Spanning Tree, MST*** 라고도 불린다.
* 가능한 spanning tree 중에서, 간선의 가중치 합이 최소인 Spanning Tree를 지칭한다.

오른쪽의 그래프는 일반적인 weighted graph이다. 그 중 spanning tree의 조건을 만족하면서 간선의 가중치의 합이 최소인 그래프를 **MST** 라고 부른다.
![Alt text](/public/post/2020_03_03_spanningtree/pic2.PNG)

그래프에서 이런 최소 신장 트리를 찾을 수 있는 대표적인 알고리즘이 존재한다.

**Kruskal's Algorithm (크루스칼 알고리즘)**

**Prim's Algorithm (프림 알고리즘)**

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 3. 크루스칼 알고리즘 (Kruskal's algorithm)

1. 모든 정점을 독립적인 집합으로 만든다.

2. 모든 간선을 비용을 기준으로 정렬하고, 비용이 작은 간선부터 양 끝의 두 정점을 비교한다. (비용이 작은 순서로 정렬한다.)

3. 두 정점의 최상위 정점을 확인하고, 서로 다를 경우 두 정점을 연결한다. (사이클이 있는지 없는지를 체크한다. 사이클이 없어야 Spanning Tree이다.)

**탐욕알고리즘을 기초로 한다**

![Alt text](/public/post/2020_03_03_spanningtree/pic3.PNG)

![Alt text](/public/post/2020_03_03_spanningtree/pic4.PNG)

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 4. Union-Find 알고리즘
크루스칼 알고리즘을 수행하기 위해 Union-Find 알고리즘을 활용한다.

**Disjoint Set이란**
* 서로 중복되지 않는 부분 집합들로 나눠진 원소들에 대한 정보를 저장하고 조작하는 자료구조

* 공통 원소가 없는 (서로소) 상호 배타적인 부분 집합들로 나눠진 원소들에 대한 자료구조를 의미한다.

* Disjoint Set = 서로소 집합 자료구조

노드들 중에 연결된 노드를 찾거나, 노드들을 서로 연결할 때(합칠 때)사용한다.

##### 1. 초기화
n개의 원소가 개별 집합으로 이뤄지도록 초기화

![Alt text](/public/post/2020_03_03_spanningtree/pic5.PNG)

##### 2. Union
두 개별 집합을 하나의 집합으로 합친다. 두 트리를 하나의 트리로 만든다. (Union-by-rank를 사용한다.)

![Alt text](/public/post/2020_03_03_spanningtree/pic6.PNG)

##### 3. Find
여러 노드가 존재할 때, 두 개의 노드를 선택해서 현재 두 노드가 서로 같은 그래프에 속하는 지 판별하기 위해 각 그룹의 최상단 원소(루트 노드)를 확인한다.

즉 아래 왼쪽의 그래프에 있는 노드 A와 오른쪽 그래프의 노드 C는 서로 루트 노드가 D와 E이므로 다른 그래프에 속해있다.
![Alt text](/public/post/2020_03_03_spanningtree/pic7.PNG)

즉, 위의 **Union** 과 **Find** 를 크루스칼 알고리즘에 활용한다.

**Find** 의 경우 먼저, 간선의 가중치를 오름차순으로 정렬 한 후 가장 작은 간선 부터 보는데 간선의 양 끝단 노드를 Find를 통해 루트노드가 같으면 포기하고 루트노드가 다르면 잇는다. 이때 이을 때 **Union** 을 사용한다.

&nbsp;
&nbsp;
&nbsp;

##### Union-Find 알고리즘의 고려할 점

* Union순서에 따라서, 최악의 경우 링크드 리스트와 같은 형태가 될 수 있다.

* 이 때는 Find/Union 시 계산량이 O(N)이 될 수 있으므로 이 문제를 해결하기 위해 **union-by-rank** , **path compression** 기법을 활용한다.

![Alt text](/public/post/2020_03_03_spanningtree/pic8.PNG)

&nbsp;
&nbsp;
&nbsp;


###### Union-by-rank 기법

**각 트리에 대해 높이(rank)를 기억해두고**

1. **Union시 두 트리의 높이(rank)가 다르면, 높이가 작은 트리를 높이가 큰 트리에 붙인다.**

즉, rank2인 트리의 루트 노드가 왼쪽과 오른쪽 그래프를 합쳤을 때의 루트노드가 되게 한다.

![Alt text](/public/post/2020_03_03_spanningtree/pic9.PNG)

2. **높이가 h - 1인 두 개의 트리를 합칠 때는 한 쪽의 트리 높이를 1증가시켜주고, 다른 쪽의 트리를 해당 트리에 붙인다.**

즉, 오른쪽과 왼쪽은 둘 다 높이가 같으므로, 왼쪽의 높이를 1증가시켜주고 합쳐준다.

![Alt text](/public/post/2020_03_03_spanningtree/pic10.PNG)

즉, 초기화시 모든 원소는 높이(rank)가 0인 개별 집합인 상태에서 하나씩 원소를 합칠 때 union-by-rank기법을 사용한다면

  * 높이가 h인 트리가 만들어지려면 높이가 **h-1 인 두 개의 트리가 합쳐져야 한다.**

  * 높이가 h-1인 트리를 만들기 위해서 최소 n개의 원소가 필요하다면 높이가 h인 트리가 만들어지기 위해선 최소 2n개의 원소가 필요하다.

따라서 union-by-rank 기법을 사용하면, union/find 연산의 시간복잡도는 O(N)이 아닌 O(logN)로 낮출 수 있다.

&nbsp;
&nbsp;
&nbsp;

###### path compression 기법
Find에서 실행한 노드에서 거쳐간 노드를 루트에 다이렉트로 연결하는 기법

Find를 실행한 노드는 이후부터는 루트노드를 한 번에 알 수 있다.

아래 그림을 보자. 왼쪽 그래프는 RANK가 3이다. 하지만, 노드 B,C,D는 다 루트 노드 A를 가지고 있다. 그러므로, B,C,D의 루트 노드를 A로 저장해놓고 있으면 루트 노드를 매번 찾을 필요가 없다.

![Alt text](/public/post/2020_03_03_spanningtree/pic11.PNG)


&nbsp;
&nbsp;
&nbsp;

###### Time complexity
Union-by-rank와 path compression 기법 사용 시 시간 복잡도는 다음 계산식을 만족한다.

**O(MlogN)**
그러므로, N이 2^65536값을 가지더라도 logN은 5이므로 상수에 가깝다고 볼 수 있다.

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;


# 5. 파이썬 코드
~~~python
parent = dict()
rank = dict()

def find(node):
    # path compression
    if parent[node] != node:
        parent[node] = find(parent[node])

    return parent[node]

def make_set(node):
    parent[node] = node
    rank[node] = 0

# union-by-rank 기법
def union(node_v, node_u):
    root1 = find(node_v)
    root2 = find(node_u)

    if rank[root1] > rank[root2]:
        parent[root2] = root1
    else:
        parent[root1] = root2

        if rank[root1] == rank[root2]:
            rank[root2] += 1

# 1.초기화
def kruskal(graph):
    mst = list()

    for node in graph['vertices']:
        make_set(node)

    # 2. 간선 weight기반 sorting
    edges = graph['edges']
    edges.sort()

    # 3. 간선 연결(사이클 없는)
    for edge in edges:
        weight, node_v, node_u = edge
        if find(node_v) != find(node_u):
            union(node_v, node_u)
            mst.append(edge)

    return mst
~~~

~~~python
kruskal(mygraph)
~~~

~~~
[(5, 'A', 'D'),
 (5, 'C', 'E'),
 (6, 'D', 'F'),
 (7, 'A', 'B'),
 (7, 'B', 'E'),
 (9, 'E', 'G')]
~~~

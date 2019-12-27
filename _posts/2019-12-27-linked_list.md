---
layout: post
title: Linked List
tags:  [algorithm-and-data-structure]
---
Queue나 stack, array와 마찬가지로 대표적인 자료구조이다. 하지만, 기존의 queue나 stack, array보다는 복잡하다. 구조 자체는 간단하나 구현할 때 헷갈리는 부분이 많다.

> **배열의 가장 큰 단점은 미리 크기를 지정** 해놔야 한다. 반면, 링크드 리스트는 미리 크기를 지정하지 않고 데이터를 추가할 수 있다. 즉, **배열의 단점을 극복한 자료구조이다.**

한국어로는 연결리스트라고도 한다.
* 배열은 순차적으로 연결된 공간에 데이터를 나열하는 데이터 구조이다.
* **링크드 리스트는 떨어진 곳에 존재하는 데이터를 화살표(pointer)로 연결해서 관리하는 데이터 구조이다.**

&nbsp;
&nbsp;

#### 링크드 리스트 기본구조의 용어
* 노드(Node) : 데이터 저장 단위(데이터 값, 포인터)로 구성되어 있다.
* 포인터(Pointer) : 각 노드 안에서, 다음이나 이전의 노드와의 연결 정보를 가지고 있는 공간.

**일반적인 링크드 리스트의 형태는 아래와 같다.**

![Alt text](/public/linked_list.png)

&nbsp;
&nbsp;

#### 링크드 리스트의 장단점(전통적인 C언어에서의 배열과 링크드 리스트)
* **장점**
  * 미리 데이터 공간을 할당하지 않아도 된다.
  * 배열은 미리 데이터 공간을 할당해야 한다.

* **단점**
  * 연결을 위한 별도의 데이터 공간이 필요하므로, 저장공간 효율이 높지 않다.
  * 연결 정보를 찾는 시간이 필요하므로 접근 속도가 느리다.
  * 중간 데이터 삭제 시, 앞 뒤 데이터의 연결을 재구성해야 하는 부가적인 작업이 필요하다.

&nbsp;
---
&nbsp;

### 파이썬으로 연결 리스트 구현하기.
Linked List는 파이썬에서 특별히 제공하지 않는다. 그래서 아래처럼 직접 구현하였다. Node class와 그 Node들을 이어서 linked list를 만드는 NodeMgmt(management) class가 있다. NodeMgmt에는 data값을 받으면 새로운 Node를 만들어 연결하는 add()함수와 연결되어 있는 모든 node를 출력하는 desc()함수, 주어진 data값과 똑같은 값을 가진 node를 삭제하는 delete()함수 마지막으로 주어진 data값과 똑같은 값을 가진 node를 반환하는 search()함수가 있다.

~~~
class Node:

    def __init__(self, data, next=None):
        self.data = data
        self.next = next

class NodeMgmt:

    def __init__(self, data):
        self.head = Node(data)

    def add(self, data):
        if self.head == None:
            self.head = Node(data)
        else:
            node = self.head
            while node.next:
                node = node.next
            node.next = Node(data)    

    def desc(self):
        node = self.head

        while node:
            print(node.data)
            node = node.next

    def delete(self, data):

        if self.head == None:
            print("해당 값을 가진 노드가 없습니다.")
            return

        if self.head.data == data:
            temp = self.head
            self.head = self.head.next
            del temp
        else:
            node = self.head
            while node.next:
                if node.next.data == data:
                    temp = node.next
                    node.next = node.next.next
                    del temp
                    return
                else:
                    node = node.next

    def search(self, data):
        node = self.head

        while node:
            if node.data == data:
                return node
            else:
                node = node.next
~~~
이제, 만든 코드를 test해보자. linked_list_1에 0으로 시작 노드를 만든 다음 1부터 9까지의 데이터를 가진 각 node를 만든다. 그리고 모두 출력한다.

~~~
linked_list_1 = NodeMgmt(0)

for data in range(1,10):
    linked_list_1.add(data)

linked_list_1.desc()

print("search ")
node_4 = linked_list_1.search(4)
print(node_4.data)
~~~

&nbsp;
#### 다양한 링크드 리스트 구조
**더블 링크드 리스트(Doubly Linked List)**
* 이중 연결 리스트라고도 한다. Node에 다음 node를 가리키는 next만 있는 것이 아니라 전의 node를 가리키는 prev도 있다.

![Alt text](/public/doubly_linked_list.png)

더블 링크드 리스트에서는 Node에 prev만 있는 것이 아니라 next도 있다. 그리고 NodeMgmt class에는 가장 처음 node를 가리키는 head와 가장 마지막 node를 가리키는 tail도 있다.

양방향으로 연결되어 있어서 노드 탐색이 양쪽으로 모두 가능 예를 들어 만약 8000번째에 원하는 data가 있다면, 앞에서 8000번 찾는 것 보단 뒤에서 2000번 찾는 게 훨씬 빠르다. 

**파이썬 코드**

~~~
class Node:

    def __init__(self, data, prev=None, next=None):
        self.data = data
        self.prev = prev
        self.next = next

class NodeMgmt:
    def __init__(self, data):
        self.head = Node(data)
        self.tail = self.head

    def insert(self, data):
        if self.head == None:
            self.head = Node(data)
            return
        else:
            node = self.head

            while node.next:
                node = node.next

            new = Node(data)
            node.next = new
            new.prev = node
            self.tail = new

    def desc(self):
        node = self.head

        while node:
            print(node.data)
            node = node.next

    def insert_before(self, data, before_data):
        if self.head == None:
            self.head = Node(data)
            return True
        else:
            node = self.tail
            while node.data != before_data:
                node = node.prev
                if node == None:
                    return False
            new = Node(data)
            before_new = node.prev
            before_new.next = new
            new.prev = before_new
            new.next = node
            node.prev = new
            return True
~~~

---
layout: post
title: 해쉬 테이블(Hash Table)
tags:  [algorithm-and-data-structure]
---
해쉬 테이블은 큐, 스택과 더블어 대표적인 자료구조 중 하나이다. 특히 요즘은 이 해쉬 테이블을 변형시켜 사용하는 데 대표적인 기술이 블록체인이다.
>**해쉬 테이블은 키(Key)에 데이터(Data)를 저장하는 데이터 구조이다. 즉 키만 알면 바로 데이터를 구할 수 있다**

그래서 키만 알고 있으면 데이터를 바로 얻을 수 있어서 속도가 아주 빠르다. 파이썬의 **딕셔너리** 가 해쉬 테이블을 사용하는 대표적인 예이다.

## Hash Table 구조

![Alt text](/public/hash_table.png)
**해쉬(Hash)** 는 임의이 값을 고정길이로 변환하는 것이다. 그래서 우리는 **key값** 을 **산술연산을 이용해 데이터 위치를 찾는 해쉬 함수에** 넣는다. 해쉬 함수의 결과값으로 구한 해쉬 값 혹은 해쉬 주소는 해쉬 테이블의 해당 주소를 가리키고 이 주소의 슬롯에 데이터를 넣거나 가져올 수 있다.

* **해쉬 (Hash)** : 임의의 값을 고정길이로 변환하는 것.
* **해쉬 테이블** : 키 값의 연산에 의해 직접 접근이 가능한 데이터 구조
* **해싱 함수** : Key에 대해 산술 연산을 이용해 데이터 위치를 찾을 수 있는 함수
* **해쉬 값 또는 해쉬 주소** : Key를 해싱 함수로 연산해서 해쉬값을 알아내고 이를 기반으로 해쉬 테이블에서 해당 Key에 대한 데이터 위치를 일관성 있게 찾을 수 있다.
* **슬롯** : 한 개의 데이터를 저장할 수 있는 공간

> **참고로 저장할 데이터에 대한 key값을 구하는 함수가 별도로 있을 수 도 있다.**

&nbsp;

## 자료구조 해쉬 테이블의 장단점

#### 장점
* 데이터를 저장하거나 읽는 속도가 빠르다. 즉, **검색 속도** 가 빠르다.
* 해쉬는 키에 대한 데이터가 있는지(중복) 확인이 쉽다.

#### 단점
* 일반적으로 저장공간이 많이 필요하다. 해쉬 테이블에서 사용되지 않는 공간까지 미리 만들어 놓아야 한다.
* **여러 키에 대해 해당하는 주소가 같을 경우 충돌을 해결하기 위한 별도의 자료구조가 필요하다.**

#### 주요 용도
* 검색이 많이 필요한 경우
* 저장, 삭제, 읽기가 빈번한 경우
* 캐쉬 구현 시 (중복 확인이 쉽기 때문, 캐쉬같은 경우 브라우저를 통해 새 사이트를 열 때 모든 데이터를 가져오는데, 매번 이렇게 데이터를 가져오는 것이 느리므로 중복되는 데이터는 캐쉬(저장)해 놓는다. 이럴 때 중복된 것이 있는지 확인할 때 해쉬테이블이 사용될 수 있다)

&nbsp;

## 파이썬 구현

1. 해쉬 함수 : key % 8
2. 해쉬 키 생성 : hash(data)

참고로 파이썬에서는 hash()라는 내장함수가 있어서 안에 데이터를 넣으면 hash값을 반환해준다. 하지만 컴퓨터를 다시 켤때마다 새롭게 update되는 단점이 있다.

~~~python
hash_table = [0 for i in range(8)] # 8개의 slot을 가진 hash table.

def get_key(data): # data를 hash()함수에 넣어 키 값을 구한다.
    return hash(data)

def hash_func(key): # 키 값이 주소로 변환이 된다.
    return key % 8

def save_data(data, value):
    hash_address = hash_func(get_key(data))
    hash_table[hash_address] = value

def read_data(data):
    hash_address = hash_func(get_key(data))
    return hash_table[hash_address]

# 테스트
save_data('Dave', '01033332222')
save_data('Andy', '010322135555')
print(read_data('Dave'))
print(hash_table)
~~~
결과값
~~~
01033332222
['01033332222', '010322135555', 0, 0, 0, 0, 0, 0]
~~~

&nbsp;

## 충돌 알고리즘(좋은 해쉬 함수 사용하기)

해쉬 테이블에서 가장 큰 문제는 **충돌** 의 경우이다. 이런 문제를 **충돌(collision) 또는 해쉬 충돌(hash collision)이라고** 부른다. 해쉬 충돌 알고리즘은 크게 2가지가 있다.

### 6.1 Chaining 기법
* 개방해싱 또는 open hashing기법 중 하나 : 해쉬 테이블 저장공간 외의 공간을 활용하는 기법
* **충돌이 일어나면 링크드 리스트의 자료구조를 사용해서 링크드 리스트로 데이터를 추가로 뒤에 연결시키는 방법.**

![Alt text](/public/hash_table_collision_chaining.png)

밑의 파이썬에서는 linked_list를 사용하진 않고 대신 list를 사용하였다.

##### python 프로그래밍
~~~python
hash_table = [0 for i in range(8)]

def get_key(data):
    return hash(data)

def hash_func(key):
    return key % 8

def save_data(data, value):
    index_key = get_key(data)
    hash_address = hash_func(index_key)
    if hash_table[hash_address] == 0: # 만약 hash_table의 hash_address에 데이터가 없으면 바로 넣는다.
        hash_table[hash_address] = [[index_key, value]]
    else: # 만약 데이터가 있다면 그 hash_address를 가진 모든 데이터를 list형식으로 넣는다.
        for index in range(len(hash_table[hash_address])):
            if hash_table[hash_address][index][0] == index_key:
                hash_table[hash_address][index][1] = value
                return
        hash_table[hash_address].append([index_key, value])

def read_data(data):
    index_key = get_key(data)
    hash_address = hash_func(index_key)

    if hash_table[hash_address] == 0:
        return None
    else:
        for index in range(len(hash_table[hash_address])):
            if hash_table[hash_address][index][0] == index_key:
                return hash_table[hash_address][index][1]
        return None
~~~

### 6.2 Linear Probing 기법
* 폐쇄 해슁 또는 Close Hashing 기법 중 하나 : 해쉬 테이블 저장공간 안에서 충돌문제를 해결하는 기법
* **충돌이 일어나면, 해당 Hash address의 다음 address부터 맨 처음 나오는 빈공간에 저장하는 기법**
* 저장공간 활용도를 높이기 위한 기법

![Alt text](/public/hash_table_collision_closed.png)

##### python 프로그래밍
~~~python
hash_table = [0 for i in range(8)]

def get_key(data):
    return hash(data)

def hash_func(key):
    return key % 8

def save_data(data, value):
    index_key = get_key(data)
    hash_address = hash_func(index_key)

    if hash_table[hash_address] == 0:
        hash_table[hash_address] = [index_key, value]
    else:
        for index in range(hash_address, len(hash_table)):
            if hash_table[index] == 0:
                hash_table[index] = [index_key, value]
                return
            elif hash_table[index][0] == index_key:
                hash_table[index][1] = value
                return

def read_data(data):
    index_key = get_key(data)
    hash_address = hash_func(index_key)

    if hash_table[hash_address] == 0:
        return None
    elif hash_table[hash_address][0] == index_key:
        return hash_table[hash_address][1]
    else:
        for index in range(hash_address, len(hash_table)):
            if hash_table[index] == 0:
                return None            
            elif hash_table[index][0] == index_key:
                return hash_table[index][1]

        return None
~~~

##### 빈번한 충돌을 개선하는 기법

> 해쉬 함수의 재정의 및 해쉬 테이블 저장공간을 확대
그 중, 가장 손쉬운 방법이 **해쉬 테이블의 저장공간을 확대** 하는 것이다.

### 참고
* 파이썬의 hash()함수는 실행할 때마다 값이 달라질 수 있다. 그래서 **유명한 해쉬함수** 들을 사용할 수 있다 : **SHA(Secure Hash Algorithm)**

* 어떤 데이터도 유일한 고정된 크기의 고정값을 리턴해주므로 해쉬 함수로 유용하게 활용 가능

##### SHA-1

~~~python
import hashlib

data = 'test'.encode() # byte로 바꿔준다.
hash_object = hashlib.sha1()
hash_object.update(data) # hash_object.update('b''test)로 써도 된다.
hex_dig = hash_object.hexdigest() # 16진수로 추출.
print(hex_dig) # 16진수로 변환한 값이 아래와 같다.
~~~

##### SHA-256
~~~python
import hashlib

data = 'test'.encode() # byte로 바꿔준다.
hash_object = hashlib.sha256()
hash_object.update(data) # hash_object.update('b''test)로 써도 된다.
hex_dig = hash_object.hexdigest() # 16진수로 추출.
print(int(hex_dig, 16)) # 16진수로 변환한 값이 아래와 같다.
~~~

### 시간복잡도

* 일반적인 경우 (collision이 없는 경우)는 O(1)
* 최악의 경우 (모든 데이터마다 collision이 있을 경우)는 O(n)

> **해쉬 테이블의 경우, 일반적인 경우를 기대하고 만들기 때문에 시간복잡도는 O(1)이다.**

##### 검색에서 해쉬 테이블의 사용 예

* 16개의 배열에 데이터를 저장하고 검색할 때 O(n)
* 16개의 데이터 저장공간을 가진 위의 해쉬 테이블에 데이터를 저장하고 검색할 때 O(1)

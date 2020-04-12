---
layout: post
title: Python Advanced Dict
tags: [python]
---
dictionary는 파이썬의 핵심 구현부이다. 왜냐하면, Class, Instance, 함수의 키워드, 주석 다큐먼트 전부 다 dictionary로 사용되기 때문이다. 특히, 파이썬이 **hashtable을 정말 잘 만들어놔서 고성능으로 사용이 가능하다.**

&nbsp;
&nbsp;
&nbsp;

# Dict Setdefault
dictionary를 사용할 댄 key값이 중복되면 안된다. 이럴 경우를 위해 사용하는 함수가 Setdefault이다.

예를 들어, 다음과 같이 score를 dictionary로 만들어보자. k1이 겹치므로 dciontary로 만들 수 없다.
~~~python
source = (('k1', 'val1'), ('k1', 'val2'), ('k2', 'val3'), ('k2', 'val4'), ('k2', 'val5'))
~~~

그래서 다음과 같은 작업을 해주어야 한다.

~~~python
new_dict1 = {}
new_dict2 = {}

# No use setdefault
for k, v in source:
    if k in new_dict1:
        new_dict1[k].append(v) # 있으면 list에 append
    else:
        new_dict1[k] = [v] # 없으면 그대로

print("Ex3-1 - ", new_dict1)
~~~

~~~
Ex3-1 -  {'k1': ['val1', 'val2'], 'k2': ['val3', 'val4', 'val5']}
~~~

이런 경우 __setdefault()__ 함수를 사용할 수 있다. setdefault함수는 키값이 있다면 키 값을 반환하고 없다면 두 번째 인자를 반환한다.

~~~python
new_dict2 = {}

for k,v in source:
    new_dict2.setdefault(k, []).append(v)

print(new_dict2)
~~~

~~~
{'k1': ['val1', 'val2'], 'k2': ['val3', 'val4', 'val5']}
~~~

# defaultdict
defaultdict은 말 그대로 dictionary의 기본 값을 정의하고 같이 없을 경우 에러를 출력하지 않고 기본 값을 출력한다. setdefault보다 더 빠르다.

그래서 key값이 없더라도 에러가 나지 않고 전에 지정한 int값이 저장된다.
~~~python
from collections import defaultdict

int_dict = defaultdict(int)

str_ex = 'appplebnana'

for c in str_ex:
    int_dict[c] += 1

print(int_dict)
~~~

~~~
defaultdict(<class 'int'>, {'a': 3, 'p': 3, 'l': 1, 'e': 1, 'b': 1, 'n': 2})
~~~

&nbsp;
&nbsp;
&nbsp;

# immutable Dict
MappingProxyType은 수정도 안되고 추가도 안되게 해준다.

~~~python
from types import MappingProxyType

d = {'key1' : 'TEST1'}

d_frozen = MappingProxyType(d)

d_frozen['key1'] = 'TEST2'
~~~

~~~
d_frozen['key1'] = 'TEST2'
TypeError: 'mappingproxy' object does not support item assignment
~~~

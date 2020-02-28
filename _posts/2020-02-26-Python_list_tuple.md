---
layout: post
title: 파이썬 list & tuple Advanced
tags:  [Python]
---

# 시퀀스형이란
시퀀스형의 자료형은 무언가의 조합으로 이루어진 자료형이다.
시퀀스형에는 다음과 같은 3가지의 기본 시퀀스형이 존재한다.
* **리스트**
* **튜플**
* **레인지**

&nbsp;
&nbsp;

### Container vs Flat
**Container :** 서로 다른 자료형을 저장할 수 있다. ex) list, tuple, collections.deque

**Flat :** 한 개의 자료형만을 저장할 수 있다. ex) str, bytes, bytearray, array.array, memoryview

**Flat은 한 개의 자료형만을 저장할 수 있기 때문에 Container보다 처리속도가 훨씬 빠르다.**

&nbsp;
&nbsp;

### 가변 vs 불변
**가변 :** 지속적으로 데이터를 추가 및 삭제하기 위해 사용 ex) list, bytearray, array.array, memoryview, deque

**불변형 :** 실수로 값을 변경하는 것을 막기 위해 사용 ex) tuple, str, bytes

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 지능형 리스트 (List Comprehension / Comprehending List)
입력 Sequence로부터 지정된 표현식에 따라 새로운 리스트 컬렉션을 빌드하는 것이다.

다음과 같은 문법을 갖는다.

**[출력표현식 for 요소 in 입력Sequence [if 조건식]]**

그러면 list comprehension을 쓰지 않을 때와 쓸 때의 차이를 알아보자.

**리스트 컴프리헨션을 사용하지 않을때**
~~~python
chars = '!@#$%^&*()_'
codes1 = []

for s in chars:
    codes1.append(ord(s))

print(codes1)
~~~

**리스트 컴프리헨션을 사용할 때**
~~~python
chars = '!@#$%^&*()_'
print([ord(s) for s in chars])
~~~

리스트 컴프리헨션을 사용하면 훨씬 코드가 짧다.

**그리고 대용량의 데이터를 처리할 때  list comprehension이 조금 더 우세하다고 한다.**

&nbsp;
&nbsp;

### list comprehension에 if문 추가

~~~python
chars = '!@#$%^&*()_'
print([ord(s) for s in chars if ord(s) > 40])

다른 방법
~~~python
print(list(map(ord, chars)))
print(list(filter(lambda x : x > 40, map(ord, chars))))
~~~

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# Geneator

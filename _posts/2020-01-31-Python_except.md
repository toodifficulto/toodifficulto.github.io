---
layout: post
title: 예외처리
tags:  [Python]
---

항상 예상치 못한 에러가 생길 수 있다. 특히 결제라는 이벤트에서 에러가 나면 회사의 입장에선 큰 손실을 얻는다. 그래서 회사에서는 언제나 에러 처리를 할 수 있는 케이스를 조사하고 대비해야 한다.

이런 에러 처리가 왜 중요하냐면 코드에서는 에러가 없더라도 하드웨어에서 나타나는 뜬금없는 오류를 완벽하게 처리할 수 없다. 그래서 예외처리를 통해서 에러가 나는 순간 내가 만들어놓은 순서대로 처리가 되게 하고 고쳐야 한다.

&nbsp;
&nbsp;
&nbsp;

# 파이썬 예외처리의 이해

### 예외의 종류
문법적으로는 에러가 없지만, 코드 실행 (런타임)프로세스에서 발생하는 예외 처리도 중요하다.
IDE에는 linter라는 게 있다. linter는 코드 스타일, 문법 체크등을 해주는 기능이다. 그래서 문법적 에러는 처리하기가 쉽다.

#### Syntax Error :잘못된 문법
~~~python
print('Test)

if True
      print('hi')

x => y
~~~

~~~
File "<ipython-input-2-9747c721ddaf>", line 1
   print('Test)
               ^
SyntaxError: EOL while scanning string literal
~~~

### NameError : 참조 변수가 없을 때 생기는 에러
~~~python
a = 10
b = 15
print(c) # 존재하지 않는 c를 호출한다.
~~~
~~~
NameError                                 Traceback (most recent call last)
<ipython-input-3-d389cda1ce8f> in <module>
      1 a = 10
      2 b = 15
----> 3 print(c) # 존재하지 않는 c를 호출한다.

NameError: name 'c' is not defined
~~~

#### ZeroDivisionError: 모든 언어에 존재하는 중요한 에러이다. 그리고 runtimeError이다.

~~~python
print(10 / 0) #문법적으로는 에러가 없지만 0으로 나눌 수 없으므로 에러가 난다.
~~~

~~~
ZeroDivisionError                         Traceback (most recent call last)
<ipython-input-4-7df43aa61c85> in <module>
----> 1 print(10 / 0) #문법적으로는 에러가 없지만 0으로 나눌 수 없으므로 에러가 난다.

ZeroDivisionError: division by zero
~~~

#### IndexError : 아주 많이 발생하는 에러이다. 인덱스 범위가 오버된 것이다.

~~~python
x = [10, 20, 30]
print(x[0])
print(x[3])
~~~

~~~
10
---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
<ipython-input-6-ea3df66951ab> in <module>
      1 x = [10, 20, 30]
      2 print(x[0])
----> 3 print(x[3])

IndexError: list index out of range
~~~

#### KeyError: 주로 딕셔너리에서 발생하는 에러이다.  없는 키를 참조하였을 때 생기는 에러이다.
~~~Python
dic = {'name' : 'kim', 'Age' : 33, 'City' : 'Seoul'}
print(dic.get('hobby')) # 아래와 같은 에러를 막기 위해 get메소드를 사용하면 키가 없을 때 None을 반환한다.

print(dic['hobby'])
~~~
~~~
None
---------------------------------------------------------------------------
KeyError                                  Traceback (most recent call last)
<ipython-input-8-00f914f4f842> in <module>
      2 print(dic.get('hobby')) # 아래와 같은 에러를 막기 위해 get메소드를 사용하면 키가 없을 때 None을 반환한다.
      3
----> 4 print(dic['hobby'])

KeyError: 'hobby'
~~~

#### AttributeError : 모듈, 클래스에 있는 잘못된 속성 사용시에 예외
~~~python
import time

print(time.time()) # time에는 time이 존재한다
print(time.month()) # time에는 month가 없다.
~~~
~~~
1580453684.3917792
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-9-7db88384170c> in <module>
      2
      3 print(time.time()) # time에는 time이 존재한다
----> 4 print(time.month()) # time에는 month가 없다.

AttributeError: module 'time' has no attribute 'month'
~~~

#### ValueError : 참조 값이 없을 때 발생
~~~Python
x = [1, 5, 9]
x.remove(10) # 리스트에는 10이 포함되어 있지 않다.
~~~
~~~
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-11-1a361d0d0eac> in <module>
      1 x = [1, 5, 9]
----> 2 x.remove(10) # 리스트에는 10이 포함되어 있지 않다.

ValueError: list.remove(x): x not in list
~~~

#### FileNotFoundError : 외부 파일을 처리할 때 발생하는 에러이다. 파일이 존재하지 않을 때 생기는 에러

~~~python
f = open('test.txt', 'r')
~~~
~~~
---------------------------------------------------------------------------
FileNotFoundError                         Traceback (most recent call last)
<ipython-input-14-9f3bddd907fb> in <module>
----> 1 f = open('test.txt', 'r')

FileNotFoundError: [Errno 2] No such file or directory: 'test.txt'
~~~


#### TypeError : 형변환 에러이다.
~~~python
x = [1,2]
y = (1,2)
z = 'test'
print(x + y) # tuple과 list는 결합할 수 없다.
~~~

~~~
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-15-682e62b43b72> in <module>
      2 y = (1,2)
      3 z = 'test'
----> 4 print(x + y)

TypeError: can only concatenate list (not "tuple") to list
~~~

항상 예외가 발생하지 않을 것이라고 가정하고 먼저 코딩을 하고 실행 때 런타임 예외 발생 시 예외처리 코딩 권장(EAP 코딩 스타일)

**일단 만들고 예외가 나면 예외처리를 해주는 방식**

&nbsp;
&nbsp;
&nbsp;

# 예외 처리 기본
**try** : 에러가 발생할 가능성이 있는 코드 실행
**except** : 에러명 1
**except** : 에러명 2
**else** : 에러가 발생하지 않았을 때 실행
**finally** : 항상 실행

~~~python
name = ['kim', 'lee', 'park']

try:
    z = 'kim'
    x = name.index(z)
    print('{} Found it! in name'.format(z, x + 1))
except ValueError:
     print('Not found it - Occured ValueError')
else: # Error가 나지 않을 때 실행된다.
    print('Ok! else!')
~~~

~~~
kim Found it! in name
Ok! else!
~~~

~~~python
name = ['kim', 'lee', 'park']

try:
    z = 'Cho' # Cho는 없으므로 에러가 난다.
    x = name.index(z)
    print('{} Found it! in name'.format(z, x + 1))
except ValueError:
     print('Not found it - Occured ValueError')
~~~

~~~
Not found it - Occured ValueError
~~~


~~~python
name = ['kim', 'lee', 'park']

try:
    z = 'kim'
    x = name.index(z)
    print('{} Found it! in name'.format(z, x + 1))
except ValueError:
     print('Not found it - Occured ValueError')
else: # Error가 나지 않을 때 실행된다.
    print('Ok! else!')
finally: # finally는 error가 나든, 안 나든 모든 상황에서 실행이 된다.
    print('finally ok')
~~~

~~~
kim Found it! in name
Ok! else!
finally ok
~~~


예를 들어서 try구문에 데이터베이스와 연결되는 코드가 있다면 연결이 되든 안 되든, 이용 후 데이터 베이스와 연결을 끊어준느 코드가 존재해야 한다. 이런 무조건적인 수행을 할 때는 finally를 사용한다.


~~~python
name = ['kim', 'lee', 'park']

try:
    z = 'Cho'
    x = name.index(z)
    print('{} Found it! in name'.format(z, x + 1))
except ValueError:
     print('Not found it - Occured ValueError')
else: # Error가 나지 않을 때 실행된다.
    print('Ok! else!')
finally: # finally는 error가 나든, 안 나든 모든 상황에서 실행이 된다.
    print('finally ok')
~~~

예외처리는 하지 않지만, 무조건 수행되는 코딩 패턴을 파이썬에서는 자주 사용한다.

~~~python
try:
    print('Try') #Except이 없어서 Error는 수행히자 않지만, 무조건 finally구문은 실행한다 .
finally:
    print('Ok Finally!!!')
~~~

### Alias

~~~python
name = ['kim', 'lee', 'park']

# 계층적으로 할 수 도 있다.
try:
    z = 'kim'
    x = name.index(z)
    print('{} Found it! in name'.format(z, x + 1))
except ValueError as l: # 이렇게 alias를 줄 수 도 있다.
     print('Not found it - Occured ValueError')
except IndexError:
     print('Not found it - Occured IndexError')
except Exception: # 마지막은 모든 에러를 처리한다.
    print('Not found it - Occured Error')
else: # Error가 나지 않을 때 실행된다.
    print('Ok! else!')
finally: # finally는 error가 나든, 안 나든 모든 상황에서 실행이 된다.
    print('finally ok')
~~~

&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 예외 발생
예외를 발생하고 싶으면 raise를 사용한다. raise키워드로 예외를 직접 발생 시킨다.

~~~python
# 예외 발생 : raise
# raise 키워드로 예외 직접 발생

try:
    a = 'Kim'
    if a == 'Kim':
        print('Okay 허가')
    else:
        raise ValueError # 예를 들어 Kim이 아닌 다른 사람이 로그인을 시도했다고 하면 Error를 내서 누군지 알아내야 한다.
except ValueError:
    print('문제 발생!')
except Exception as f:
    print(f)
else:
    print('Okay')
~~~

~~~
Okay 허가
Okay
~~~

~~~python
# 예외 발생 : raise
# raise 키워드로 예외 직접 발생

try:
    a = 'Cho'
    if a == 'Kim':
        print('Okay 허가')
    else:
        raise ValueError # 예를 들어 Kim이 아닌 다른 사람이 로그인을 시도했다고 하면 Error를 내서 누군지 알아내야 한다.
except ValueError:
    print('문제 발생!')
except Exception as f:
    print(f)
else:
    print('Okay')
~~~

~~~
문제 발생!
~~~

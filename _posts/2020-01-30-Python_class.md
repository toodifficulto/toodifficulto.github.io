---
layout: post
title: 파이썬의 클래스
tags:  [python]
---

# 클래스를 사용하는 이유
프로그램의 덩치가 커지면 클래스 방식으로 코딩을 해서 구조화해야 한다. 이런 방식은 유지보수를 하는데 훨씬 효율적이다. 또한 메소드나 데이터의 반복을 없애주고 재활용을 할 수 있는데 이 방법을 객체지향 프로그래밍이다.

&nbsp;
&nbsp;
&nbsp;

# 클래스 사용
### 클래스 선언하는 방법
~~~python
class 클래스명 :
  함수
  함수  
  함수
~~~

클래스는 속성과 메소드로 이루어 진다. 메소드는 행동을 정의해놓은 것이라고 생각하면 쉽고 속성은 말 그대로 속성을 정의한 것이라고 생각하면 된다.

클래스를 초기화 할땐 __init__ 함수를 정의해야 한다.

**예시**
~~~python
class UserInfo:
    # 클래스는 속성과 메소드로 이루어 진다.
    def __init__(self, name): #클래스를 초기화 할때 사용되는 함수
        print('초기화', name)        
~~~
&nbsp;

### 클래스 호출
~~~python
# 자동으로 호출이 된다.
user1 = UserInfo()
~~~
&nbsp;
&nbsp;
&nbsp;
&nbsp;

# 클래스와 인스턴스의 차이

클래스(객체)를 만든 다음 변수에 클래스를 할당시켜서 인스턴스화 하여 사용한다.

네임스페이스 : 객체를 인스턴스화 할 때 저장된 공간

**예시**
~~~python
class UserInfo:
    # 클래스는 속성과 메소드로 이루어 진다.

    def __init__(self, name): #속성을 저장하는 방법은 클래스를 초기화할때 유닛들을 입력받는다.
        self.name = name

    def user_info_p(self):
        print('Name : ', self.name)

user1 = UserInfo("Kim")
user1.user_info_p()

user2 = UserInfo('Park')
user2.user_info_p()

# instance는 무엇인가? user1과 user2는 다르다.
# user1의 namespace를 찍어보면
print(user1.__dict__)

# user2에는 당연히 Park이 있다.
print(user2.__dict__)

# 각각의 메모리 장소도 다르다.
print(id(user1))
print(id(user2))
~~~

~~~
Name :  Kim
Name :  Park
{'name': 'Kim'}
{'name': 'Park'}
2273767528768
2273767529048
~~~

&nbsp;
&nbsp;
&nbsp;

# self의 이해
클래스에서는 클래스 변수와 인스턴스 변수가 존재하고 클래스 함수와 인스턴스 변수가 존재한다. 클래스 함수/변수와 인스턴스 함수/변수를 나누는 가장 큰 특징은 **self** 의 사용유무 이다.

**클래스 변수** : 직접 사용 가능, 객체보다 먼저 생성

**클래스 메소드** : 여러 인스턴스가 공유한다.

**인스턴스 변수** : 객체마다 별도로 존재, 인스턴스 생성후 사용한다.

**인스턴스 메소드** : 인스턴스만 호출이 가능하다.

먼저 아래처럼 SelfTest라는 클래스를 만들었다고 하자. 그리고 function1에는 self인자를 주지 않고 function2에는 self인자를 주었다. fucntion1은 클래스 메소드가 되고 function2는 인스턴스 메소드가 된다.

~~~python
class SelfTest:
    def function1(): # 클래스 메소드 여러 인스턴스가 공유한다.
        print('function1 called!')

    def function2(self): # 인스턴스 메소드
        print(id(self))
        print('function2 called!')

self_test = SelfTest()
~~~

그리고 self_test에서 function1을 호출하면 error가 뜬다. 왜냐하면 function1은 클래스 메소드로 인스턴스가 호출하지 못하기 대문이다.

~~~python
self_test.function1()
~~~

~~~
TypeError                                 Traceback (most recent call last)
<ipython-input-47-d1db8593f078> in <module>
      1 self_test = SelfTest()
----> 2 self_test.function1() # self인자가 없다. 즉, function1은 클래스 메소드이다. 이럴 땐 클래스에서 호출해야 한다.
      3 # SelfTest.function1()
      4 # self_test.function2()

TypeError: function1() takes 0 positional arguments but 1 was given
~~~

그래서 function1을 호출하기 위해선 객체인 SelfTest를 사용해야 한다. 반대로 function2는 인스턴스 함수이므로 self_test가 호출할 수 있다.

~~~python
SelfTest.function1()
self_test.function2()
~~~

~~~
function1 called!
2273767905208
function2 called!
~~~        

function2를 객체에서 호출하기 위해선 인스턴스인 self_test를 인자로 주어야 한다.
~~~python
SelfTest.function2(self_test) # 이렇게 실행하면 error가 나지 않는다.
~~~

### 클래스 변수, 인스턴스 변수

WareHouse에서 stock_num은 클래스 변수로 self가 없다. 그리고 모든 isntance가 stock_num을 공유한다. self.name은 인스턴스 변수로 각 인스턴스마다 가지고 있다.

~~~python
class WareHouse:
    # 클래스 변수는 self가 없다.
    stock_num = 0 #self가 없기 때문에 여러 instance에서 공유를 한다.

    def __init__(self, name):
        self.name = name
        WareHouse.stock_num += 1

    def __del__(self):
        WareHouse.stock_num -= 1
~~~

각 instance에는 인스턴스 변수인 name이 저장되어 있지만 클래스 변수인 stock_num은 없다.

~~~python
user1 = WareHouse('Kim')
user2 = WareHouse('Park')
user3 = WareHouse('Lee')

# 각 instance안에 인스턴스 변수인 name이 저장되어 있다. 하지만, 클래스 변수인 stock_num은 없다.
print(user1.__dict__)
print(user2.__dict__)
print(user3.__dict__)
~~~
~~~
{'name': 'Kim'}
{'name': 'Park'}
{'name': 'Lee'}
~~~

WareHouse의 stock_num을 보면 인스턴스가 만들어질 때마다 stock_num에 1을 추가해주었으므로 3이 된것을 볼 수 있다.
~~~python
# 3명이 사용하고 있는 것을 확인할 수 있다.
print(WareHouse.__dict__) #클래스 네임 스페잇, 클래스 변수 (공유)
~~~

~~~
{'__module__': '__main__', 'stock_num': 3, '__init__': <function WareHouse.__init__ at 0x00000211670A20D0>, '__del__': <function WareHouse.__del__ at 0x00000211670A2A60>, '__dict__': <attribute '__dict__' of 'WareHouse' objects>, '__weakref__': <attribute '__weakref__' of 'WareHouse' objects>, '__doc__': None}
~~~

또한 del을 했을 때 stock_num을 -1해주라고 만들었으므로 user1을 del해주고 나서 stock_num을 보면 -1이 된 2인 것을 볼 수 있다.
~~~python
del user1
print(user2.stock_num)
~~~

~~~
2
~~~

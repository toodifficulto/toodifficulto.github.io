---
layout: post
title: 파이썬 클래스 Advanced(2) 클래스 변수와 인스턴스 변수
tags:  [Python]
---
# 클래스 변수와 인스턴스 변수
**클래스 내부에 선언된 변수를 클래스 변수** 라고 하며, self.name과 같이 **self가 붙어 있는 변수를 인스턴스 변수라고 한다.** 클래스 변수는 Account 클래스의 네임스페이스에 위치하며, self.name과 같은 인스턴스 변수는 인스턴스의 네임스페이스에 위치하게 된다.

&nbsp;
&nbsp;

##### docstring 달기
docstring이란 클래스나 메소드를 설명 해주기 위해 달아놓는 주석이다. 큰따음표 3개나 작은 따음표 3개를 사용하고 필드에서는 주로 클래스 바로 밑에 혹은 메소드 바르 밑에 주석을 달아 놓는다.

아래 Student 클래스를 보면 인스턴스 변수는 self가 붙어있다. 반면 self가 없는 student_count는 클래스 변수이다.

&nbsp;

~~~python
# 클래스 재 선언
class Student():
    # docString 달기
    # 클래스 주석을 만들땐 큰따음표 3개나 작은 따음표 3개를 사용한다. 보통 필드에서는 클래스 바로 밑에 혹은
    # 메소드 바로 밑에 이 클래스는 어떤 클래스인지 주석을 달아 놓는다.

    """
    Student Class
    Author : Kim
    Date : 2020.02.21
    """

    # 클래스 변수
    student_count = 0


    def __init__(self, name, number, grade, details, email=None):
        # self가 붙는 변수들은 인스턴스 변수이다.
        # self가 없고 메소드 바깥쪽에서 scope(영역)을 가지는 변수를 클래스 변수이다.
        # 클래스 변수에 접근하기 위해선 해당 앞에 클래스에 점을 찍고 접근한다.

        self._name = name
        self._number = number
        self._grade = grade
        self._details = details
        self._email = email

        # 학생 인스턴스가 만들어 질 때 마다 클래스 변수인 student_count가 1씩 증가한다.
        Student.student_count += 1

    def __str__(self):
        return 'str {}'.format(self._name)

    def __repr__(self):
        return 'repr {}'.format(self._name)

    def detail_info(self):
        print('Current Id : {}'.format(id(self)))
        print('Student Detail Info : {} {} {}'.format(self._name, self._email, self._details))

    def __del__(self):
        # 학생이 전학을 가거나 사라지면 count를 1씩 증감해준다.
        Student.student_count -= 1
~~~

&nbsp;
&nbsp;
&nbsp;

# self의 의미
아래코드를 보면 인스턴스를 클래스로 찍어낸다. 그래서 각 instance는 id값이 서로 다른 인스턴스이다.

~~~python
# 붕어빵 기계로 찍어낸것처럼 인스턴스를 클래스로 찍어낸다. 각 instance는 id값이 서로 다른 인스턴스이다.
stud1 = Student('Cho', 2, 3, {'gender' : 'Male', 'score1' : 60, 'score2' : 44})
stud2 = Student('Chang', 4, 1, {'gender' : 'Female', 'score1' : 30, 'score2' : 50})
~~~

그래서 id값을 출력해보면 다른 것을 알 수 있다.
~~~python
# 컴퓨터에서 메모리에 저장을 할 떄 각자 다른 방에 stud1와 stud2르 저장하고 있다.
print(id(stud1))
print(id(stud2))
~~~

~~~
44605768
44606056
~~~

아래 코드는 에러가 난다. 왜냐하면 self가 없기 때문이다. class는 하나의 틀이고 self는 인스턴스인데 인스턴스화 하지 않으니 self가 없고 self가 없으니 detail_info()에 self값을 줄 수 없기 때문이다.

~~~python
Student.detail_info()
~~~

그래서 아래처럼 instance를 매개변수로 넣어주면 클래스에서 인스턴스 메서드에 접근이 가능하다.

~~~python
Student.detail_info(stud1)
Student.detail_info(stud2)
print()
print()
~~~

**인스턴스 변수 직접 접근 (PEP 문법적으로 권장하지 않는다.)**

이렇게 변수에 직접적으로 접근하면 언제든지 바꿀 수 있다. 그래서 이것을 방지하기 위해 캡슐화해야 한다.

~~~python
# 그래서 private해야 한다.
stud1._name = 'HAHA'
print(stud1._name)
print()
print()

# 클래스 변수
# 접근 누구나 접근이 가능하다.
print(stud1.student_count)
print(stud2.student_count)
print(Student.student_count)
print()
print()
~~~

#### 인스턴스 네임스페이스에 없으면 상위에서 검색한다.
#### 즉 동일한 이름으로 변수 생성 가능(인스턴스 검색 후 -> 상위(클래스 변수, 부모 클래스 검색))


&nbsp;
&nbsp;

### is와 == 의 차이
is는 id가 같은 지를 체크한다. 반면 ==는 값이 같은지를 체크한다.

~~~python
a = 'abc'
b = 'abc'
print(a == b)
print(a is b) # reference label이 같은 지 체크한다 .
~~~

**그래서 id를 체크하는 것이 항상 시작이다.**

&nbsp;
&nbsp;


#### dir & dict 확인

dir로 하면 모든 정보가 다 나온다. 모든 속성값을 다 보여준다.
~~~python
print(dir(stud1))
print(dir(stud2))
~~~

~~~
['__class__', '__del__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_details', '_email', '_grade', '_name', '_number', 'detail_info', 'student_count']

['__class__', '__del__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_details', '_email', '_grade', '_name', '_number', 'detail_info', 'student_count']
~~~

dict 모든 정보가 나오지 않고 간략하게만 나오지만 값을 보여준다. 용도에 맞게 사용한다. 정말 많이 사용된다.


~~~python
print(stud1.__dict__)
print(stud2.__dict__)
~~~

~~~
{'_name': 'Cho', '_number': 2, '_grade': 3, '_details': {'gender': 'Male', 'score1': 60, 'score2': 44}, '_email': None}
{'_name': 'Chang', '_number': 4, '_grade': 1, '_details': {'gender': 'Female', 'score1': 30, 'score2': 50}, '_email': None}
~~~

&nbsp;
&nbsp;
&nbsp;

### Doctring
만약 주석이 달려있다면 주석을 볼 수 도 있다.

~~~python
print(Student.__doc__)
~~~

~~~
Student Class
Author : Kim
Date : 2020.02.21
~~~

---
layout: post
title: 파이썬 클래스 Advanced(3) Class, Instance, Static Method
tags:  [Python]
---
# Class, Instance, Static Method
변수에 클래스 변수와 인스턴스 변수가 있었다면 메소드에는 클래스 메소드 인스턴스 메소드 그리고 스태틱 메소드가 있다.

이 세가지 메소드는 모두 클래스 안에서 정의된다.

**인스턴스 메소드**  는 인스턴스를 통해서 호출이 되고 첫 번째 인자로 인스턴스 자신을 자동으로 전달한다. 관습적으로 이 인수를 **self** 로 칭한다.

**클래스 메소드** 는 클래스를 통해서 호출이 되고 **@classmethod** 라는 데코레이터를 정의된다. 첫 번째 인자로는 클래스 자신이 자동으로 전달 되고 관습적으로 **cls** 라고 칭한다.

**스태틱 메소드** 는 클래스 안에서 정의되어 클래스 네임스페이스 안에는 있을 뿐 일반 함수와 전혀 다를 게 없다. **하지만 앞 서 두 메소드와는 다르게 인자로 인스턴스나 클래스를 받지 않는다.** 하지만 클래스와 연관이 있는 함수를 클래스 안에 정의하여 클래스나 인스턴스를 통해서 호출할 때 좀 더 편하게 사용이 가능하다.

&nbsp;
&nbsp;
&nbsp;
&nbsp;

# class Method

* **class 자체를 매개변수 (cls) 로 받는다.**
* **@clasmethod** 데코레이터를 사용한다.
* **생성자 오버로딩** 에 사용된다. 잘 만들어진 웹이나 어플리케이션을 보면 instance Method 보다는 class Method로 **constructor** 를 만드는 것을 선호하는 경향이 있다. 

Employee 클래스가 있다고 하자. 이 클래스에는 2개의 클래스 변수가 있다. (workplace와 salary_per) 클래스 메소드는 이런 클래스 변수를 바꾸기 위해 사용된다. **raise_per** 을 보면 인자로 cls를 받고 클래스 변수인 cls.salary_per을 바꿔준다.

클래스 메소드는 생성자를 오버로딩할 수 도 있다. Employee에는 init 인스턴스 함수가 생성자 역할을 하는데 **employee_const** 라는 클래스 메소드를 만들어서 init함수를 오버로딩 해준다.

~~~python
class Employee:
    """
    Employee Class
    Author : Bae
    Date : 2020.02.23
    Description : Class, Instance, Static Method
    """

    # 클래스 변수
    workplace = 'Co Bank'
    salary_per = 1

    # instance method
    # 생성자
    def __init__(self, name, id, salary, position):
        self._name = name
        self._id = id
        self._salary = salary * Employee.salary_per
        self._position = position

    # class method
    # 생성자 오버로딩
    @classmethod
    def employee_const(cls, name, id, salary, position):
        return cls(name, id, salary, position)

    # class method    
    @classmethod
    def raise_per(cls, per):
        if per <= 1:
            return 'percentage should be larger than 1.'

        cls.salary_per = per

        return 'Succeeded! salary percetange modified.'

    def __str__(self):
        return 'name : {} salary : {}'.format(self._name, self._salary * Employee.salary_per)

# instance method 생성자 호출
employee_1 = Employee('Bae', 1, 10000, 'newbie')

# class method 생성자 호출
employee_2 = Employee.employee_const('Kim', 2, 20000, 'leader')

print(employee_1)
print(employee_2)
print('\n')

# salary percentage바꾸기.
Employee.raise_per(1.2)
print(employee_1)
~~~

아래 결과를 보면 class method 생성자나 instance method 생성자로 인스턴스가 잘 만들어진 것을 볼 수 있다. 그리고 class method로 만든 raise_per 메소드도 잘 작동한다.

~~~
name : Bae salary : 10000
name : Kim salary : 20000


name : Bae salary : 12000.0
~~~

&nbsp;
&nbsp;
&nbsp;
&nbsp;

# Instance Method

* **인스턴스를 매개변수(self)로 받는다.**

아래처럼 init 함수나 str함수같이 self를 매개변수로 받는 함수를 인스턴스 함수라고 한다.

~~~python
def __init__(self, name, id, salary, position):
    self._name = name
    self._id = id
    self._salary = salary * Employee.salary_per
    self._position = position

def __str__(self):
    return 'name : {} salary : {}'.format(self._name, self._salary * Employee.salary_per)
~~~

&nbsp;
&nbsp;
&nbsp;
&nbsp;

# Static Method

* **위 2개 메소드와는 달리 cls나 self를 매개변수로 받지 않는다.**

* self도 필요없고 cls도 필요 없으면서 클래스와 관련되어 클래스를 위해 작동하는 메소드이다.

* **@staticmethod 데코레이터를 사용한다.**

* static Method를 호출하는 방법은 instance method를 호출하는 방식과 class method를 호출하는 방식 2가지 다 가능하다.   

아래 예시를 보면 static method인 winner_salary는 데코레이터 @staticmethod를 받는다. 그리고 매개변수로 cls나 self를 받지 않는다.

static method를 호출하는 방식은 instance method를 호출하는 방식과 class method를 호출하는 방식 2가지 다 사용가능 하다.

~~~python
@staticmethod
def winner_salary(inst):
    if inst._salary > 10000:
        return 'salary : {} Winner Salary!'.format(inst._salary)
    else:
        return "salary : {} Let's Work Harder!".format(inst._salary)

# static Method 사용
# instance method 호출 방식
print(employee_1.winner_salary(employee_1))
print(employee_2.winner_salary(employee_2))
print('\n')

# class method 호출 방식
print(Employee.winner_salary(employee_1))
print(Employee.winner_salary(employee_2))
~~~

~~~
salary : 10000 Let's Work Harder!
salary : 20000 Winner Salary!


salary : 10000 Let's Work Harder!
salary : 20000 Winner Salary!
~~~

출저 : 패트스캠퍼스 파이썬 웹개발, [초보몽키의 개발공부로그](https://wayhome25.github.io/cs/2017/04/05/cs-07/)

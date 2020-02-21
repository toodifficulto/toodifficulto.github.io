---
layout: post
title: 파이썬 클래스 Advanced(1)
tags:  [Python]
---

# 객체지향 VS 절차지향
컴퓨터 프로그래밍을 하기 위한 언어에는 많은 종류가 있는데 이러한 언어를 크게 두 가지로 나누어진다. 바로 **객체지향** 언어와 **절차지향** 언어이다.

**절차지향 언어** 는 **순차적인 처리** 가 중요시 되며 프로그램 전체가 유기적으로 연결되도록 만드는 프로그램 기법이다. 대표적인 절차지향 언어는 **C언어** 이다. 이는 컴퓨터 작업 처리방식과 유사하기 때문에 **객체지향 언어를 사용하는 것에 비해 컴퓨터의 작업 처리가 빠르다**.

옛날에는 하드웨어와 소프트웨어의 개발 속도차이가 크지 않았으나 현재는 소포트웨어의 발달으로 하드웨어가 소프트웨어를 따라오지 못하게 되었다.

이는 **객체지향 언어** 가 등장하게 된 배경이 된다. **객제지향** 의 정의를 살펴보면 **실제 세계를 모델링하여 소프트웨어를 개발** 하는 방법이다. 객제지향 프로그래밍에서는 데이터와 절차를 하나의 덩어리로 묶어서 생각하게 된다.

### 객체지향의 3대 특성

##### 1. 캡슐화
캡슐화란 관련된 데이터와 알고리즘이 하나의 묶음으로 정리된 것으로 관련된 코드와 데이터가 묶여있고 오류가 없어 사용이 편리하다.

##### 2. 상속
**상속** 은 이미 작성된 클래스를 이어 받아서 새로운 클래스를 생성하는 기법이다.

##### 3. 다형성
**다형성** 이란 하나의 이름(방법)으로 많은 상황에 대처하는 기법이다. 개념적으로 동일한 작업을 하는 함수들에 똑같은 이름을 부여할 수 있으므로 코드가 더 간단해지는 효과가 있다.

&nbsp;
&nbsp;
&nbsp;

# 객체지향과 절차지향의 차이

**절차지향** 은 위에서 부터 아래로 실행되는 형태이다. 컴퓨터의 실행구조와 유사하여 속도가 아주 빠르다.

단점은 **유지보수가 어렵다.** 코드를 파악하는데 어렵고 담당자가 바뀌면 문제가 생긴다. 또한 **디버깅도 어렵다.**

x-ray나 CT같은 기계는 절차지향으로 많이 사용된다. 단순한 프로그래밍 또한 절차지향을 사용한다.

그러나 **범용적으로 데이터를 많이 사용하는 경우에는 클래스기반으로 만드는 것이 좋다.**

&nbsp;

### 절자치향 방식 코딩
예를 들어 학생 한명을 절챠지향 방식으로 코딩한다고 해보자.

~~~Python
# 학생 1
# 학생 한명을 표현하는데도 아래와 같은 많은 변수가 있다.
student_name_1 = 'kim'
student_number_1 = 1 # 1번
student_grade_1 = 1 # 1학년
student_detail_1 = [
    {'gender' : 'Male'},
    {'score1' : 95},
    {'score2' : 88}
]

# 학생 2
# 학생 한명을 표현하는데도 아래와 같은 많은 변수가 있다.
student_name_2 = 'Lee'
student_number_2 = 2 # 2번
student_grade_2 = 2 # 2학년
student_detail_2 = [
    {'gender' : 'Female'},
    {'score1' : 77},
    {'score2' : 90}
]


# 학생 1
# 학생 한명을 표현하는데도 아래와 같은 많은 변수가 있다.
student_name_3 = 'Park'
student_number_3= 3 # 3번
student_grade_3 = 4 # 4학년
student_detail_3 = [
    {'gender' : 'Male'},
    {'score1' : 90},
    {'score2' : 100}
]
~~~

**학생 3명을 만드는 데도 저렇게 일일히 변수를 만들어주어야 한다.**

&nbsp;

### 리스트 구조
리스트 구조로 만들어보자.

**참고로 직접 아래와 같이 리스트나 딕셔너리를 타이핑하는 방법으론 해선 안된다.**

~~~python
student_names_list = ['Kim', 'Lee', 'Park']
student_numbers_list = [1,2,3]
student_grades_list = [1,2,4]
student_details_list = [
    {'gender' : 'Male', 'score1' : 95, 'score2' : 88},
    {'gender' : 'Female', 'score1' : 77, 'score2' : 90},
    {'gender' : 'Male', 'score1' : 90 ,'score2' : 100}
]

~~~

학생 삭제을 해본다고 하자. 예를 들어 2번 학생이 전학을 갔다고 하면 아래와 같이 index를 맞춰서 없애주어야 한다.
이는 **관리가 아주 불편하다.**

~~~python
# 데이터의 정확한 위치(index)매핑해서 사용
del student_names_list[1]
del student_numbers_list[1]
del student_grades_list[1]
del student_details_list[1]

print(student_names_list)
print(student_numbers_list)
print(student_grades_list)
print(student_details_list)
~~~

&nbsp;


### 딕셔너리 구조
리스트보단 좀 더 편해보이고 더 많이 사용된다. 그러나 코드 반복이 지속되고 있다. 그리고 중첩 문제가 있다. **원래 JSON 형태의 데이터는 아래와 같이 담아서 사용한다.**  Django의 ORM으로 사용해서 데이터를 가지고 올때도 아래처럼 사용한다.  그러나 아직까지도 INDEX를 사용해서 삭제를 해야 한다.


~~~python

students_dicts = [
    {'student_name' : 'Kim', 'student_number' : 1, 'student_grades' : 1, 'student_details' : {'gender' : 'Male', 'score1' : 95, 'score2' : 88}},
    {'student_name' : 'Lee', 'student_number' : 2, 'student_grades' : 2, 'student_details' : {'gender' : 'Female', 'score1' : 80, 'score2' : 90}},
    {'student_name' : 'Park', 'student_number' : 3, 'student_grades' : 4, 'student_details' : {'gender' : 'Male', 'score1' : 90, 'score2' : 100}}
]

del students_dicts[1]
print(students_dicts)
print()
print()
~~~

파이썬에서 써드파티를 사용할때 딕셔너리를 이용해서 데이터를 내려준다.

써드파티란 예를 들어 DB를 MySQL이나 Oracle을 사용하는 것처럼 외부의 것을 지칭한다.

그래서 딕셔너리 형태는 눈여겨 봐야 하지만 직접 데이터를 만들 때에는 사용하는 것을 추천하지 않는다.

&nbsp;
&nbsp;
&nbsp;


# 클래스 구조
클래스 구조 설계 후 재사용성 증가, 코드 반복 최소화, 메소드 활용의 이점이 있다.

기능추가나 속성 추가를 일일히 학생마다 해 줄 필요없이 클래스에 속성이나 기능만을 수정하여 모든 인스턴스에 속성이나 기능을 추가할 수 있다.

&nbsp;


##### 클래스 기반의 큰 장점이다. 재사용성이 증가한다. ==> 확장하기 쉽다.   

~~~python
# 모든 클래스는 object를 상속받는다. 그러므로 아래와 같이 쓸 수 있다.
# class Student:
# class Student(object):
# class Student():

class Student():

    # 생성자
    def  __init__(self, name, number, grade, details):
        self._name = name
        self._number = number
        self._grade = grade
        self._details = details

    # 개발자 입장에서 이 객체가 어떤것인지, 뭐가 들어가 있는 지, 편하게 참고하기 위해서 str 메소드를

    # 오버라이드 한다. 그래서 __str__메소드를 오버라이드 해놓으면 파이썬이 알아서 print()할 때 설정해놓은 값을 출력한다.

    def __str__(self):
        return 'str : {}'.format(self._name)

    def __repr__(self):
        return 'repr : {} - {}'.format(self._name, self._number)

# 기능추가나 속성 추가를 일일히 학생마다 해 줄 필요없이 클래스에 속성이나 기능만을 수정하여 모든 인스턴스에 속성이나 기능을 추가할 수 있다.  ==> 클래스 기반의 큰 장점이다. 재사용성이 증가한다. ==> 확장하기 쉽다.   

student1 = Student('Kim', 1, 1, {'gender' : 'Male', 'score1' : 95, 'score2' : 88})
student2 = Student('Lee', 2, 2, {'gender' : 'Female', 'score1' : 77, 'score2' : 92})
student3 = Student('Park', 3, 4, {'gender' : 'Male', 'score1' : 99, 'score2' : 100})

# 모든 객체는 __dict__을 가지고 있다. __dict__은 instance의 모든 속성이 있는 namespace를 가지고 있다.
print(student1.__dict__)
print(student2.__dict__)
print(student3.__dict__)
~~~

아래처럼 리스트로 선언해보자. 그리고 객체를 print()해보면 str함수가 출력된다. 이렇게 이미 정의되어 있던 method를 **다시 오버라이딩 하는 것이 중요하다**.

~~~python
# 리스트 선언
students_list = []

students_list.append(student1)
students_list.append(student2)
students_list.append(student3)

print()

# print를 해보면 객체가 리스트에 들어가 있는 것을 볼 수 있다.
print(students_list)

for x in students_list:
    # 객체가 출력되지 않고 __str__이 출력된다.
    # __str__을 삭제하고 출력해보면 __repr__이 출력된다.
    # 우선순위는 str이 repr보다 높다.
    # 이렇게 이미 정의되어 있는 method들을  다시 오버라이딩하는 것이 정말 중요하다.
    print(x)
    print(repr(x))
~~~

&nbsp;
&nbsp;
&nbsp;


# 정리

**표현하고자 하는 것을 클래스 기반으로 했을 때 재사용성등의 장점을 가질 수 있다. 실제 세상에 존재하는 학생을 모델링 하는 것이 클래스기법이다.**

그러나 절차지향 코딩도 많이 사용된다. 예를 들어 라즈베리파이에서 온도를 측정하는 센서를 사용하는 경우 절차지향의 코딩이 훨씬 빠르고 직관적이다. 단순한 작업일 경우 절차지향 코딩이 더 효과적일 수 있다.

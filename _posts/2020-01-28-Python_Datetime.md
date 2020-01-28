---
layout: post
title: 파이썬의 시간과 날짜 모듈 알아보기
tags:  [python]
---

### 표준 모듈

파이썬에서는 파일을 찾거나 시간을 확인하거나 난수를 생성하거나 그런 일들을 하는 모듈이 존재하는데 이를 **표준 라이브러리** 라고 한다.

표준 라이브러리에서 시간과 날짜 모듈에 대해서 알아보자.

&nbsp;
&nbsp;

### 시간과 날짜 표준 라이브러리 모듈 이름 : Datetime

![Alt text](/public/post/2020_01_28_python_Datetime/instance.PNG)


datetime 모듈을 사용하면 **문자열로 된 날짜를 datetime 객체로 변환하고, 반대로 변환할 수도 있다**

주차 계산, 요일 계산등을 쉽게 할 수 있어서 **시계열 분석** 에 활용 할 수 있다.

&nbsp;
&nbsp;

###date 객체는 날짜(년, 월, 일)을 취급한다.

![Alt text](/public/post/2020_01_28_python_Datetime/date_1.PNG)

~~~python
from datetime import date

newyearsday = date(2017, 6, 3)

print(newyearsday)

# newyearsday 객체의 year, month, day를 출력할 수 있다.
print(newyearsday.year, newyearsday.month, newyearsday.day)

# isoformat의 형태로 출력
print(newyearsday.isoformat())

# 주어진 format으로 문자열 변환 시키고 반환한다.
print(newyearsday.strftime('%Y/%m/%d'))

# (%a)는 요일을 의미한다.
print(newyearsday.strftime('%Y/%m/%d (%a)'))

# 오늘의 날짜를 반환한다.
print(date.today())
~~~

~~~
2017-06-03
2017 6 3
2017-06-03
2017/06/03
2017/06/03 (Sat)
2020-01-28
~~~

&nbsp;
&nbsp;

### datetime
datetime 객체는 시각을 다룬다. 시분초뿐만 아니라 마이크로초도 포함된다.

![Alt text](/public/post/2020_01_28_python_Datetime/datetime.PNG)

~~~python
from datetime import datetime

today = datetime.today()

# 현재의 날짜.
print(today.date())

# 현재의 시간
print(today.time())

print(today.isoformat())

# 현재 시간을 주어진 포맷 형태로
print(today.strftime("%Y-%m-%d %H:%M:%S"))
~~~

~~~
2020-01-28
13:21:42.322679
2020-01-28T13:21:42.322679
2020-01-28 13:21:42
~~~

&nbsp;
&nbsp;

### 날짜 변경 포맷 형식

strftime() 메소드를 사용하면 datetime 객체를 문자열로 변경할 수 있다.
변경시에 사용될 format을 정리하였다.

![Alt text](/public/post/2020_01_28_python_Datetime/format.PNG)

&nbsp;
&nbsp;

### Dateutil 모듈
Dateutil에서 relativeDelta를 사용해서 날짜간 차이등을 구할 수 있다.

from dateutil.relativedelta import relativedelta
from datetime import datetime, date

now = datetime.now()

print(now)
# 현재 기준에서 한 달 뒤를 알려준다.
print(now + relativedelta(months = +1))

~~~python
from dateutil.relativedelta import relativedelta
from datetime import datetime, date

now = datetime.now()

print(now)
# 현재 기준에서 한 달 뒤를 알려준다.
print(now + relativedelta(months = +1))

# 현재 기준에서 한 달 전 그리고 한 주 후를 알려준다.
print(now + relativedelta(months=+1, weeks=-1))
~~~

~~~
2020-01-28 13:25:20.562582
2020-02-28 13:25:20.562582
2020-02-21 13:25:20.562582
2020-02-01 13:25:20.562582
2020-05-07
relativedelta(years=-2, months=-9, days=-24)
~~~

&nbsp;
&nbsp;

### time 모듈
time 모듈의 sleep 메소드를 사용하면 프로그램의 동작을 정해진 시간만큼 정지할 수 있다.

데이터 수집 시 프로그램의 동작을 잠시 정지하고 싶을 때 사용한다.

~~~python
from time import sleep

for x in range(0, 10):
    print(x)
    sleep(1)
~~~

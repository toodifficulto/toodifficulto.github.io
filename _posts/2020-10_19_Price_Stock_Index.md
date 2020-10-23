---
layout: post
title: 1. 주식과 지수 (파이썬을 활용한 금융공학 레시피)
tags:  [quant]
---


# 주식가격이란?
주식가격은 기업의 과거, 현재, 미래를 반영한 종합 분석의 산물이다. 

여러 분석을 통해 현재 주가가 싸다고 생각하면 주식을 사고, 비싸다고 생각하면 주식을 팔면 된다. 트레이딩 용어로는 주식을 사는 것(매수, Buy)을 **롱(long)**, 주식을 파는 것(매도, Sell)은 **숏(short)**이라고 한다. 

이렇게 서로 다른 판단을 하는 사람들이 거래소에 모여 거래가 이루어지고, 주가는 더 많은 배팅을 하는 방향으로 움직인다. 

&nbsp;
&nbsp;
&nbsp;

# 밸류에이션
주식평가에 사용되는 밸류에이션 모델은 배당할인 모형(DDM), 고든(Gordon)의 성장률 모형, CAPM, FV/EBITDA, PER, EPS, PBR등 *재무 비율을 이용한 상대비교 모형등 다양하다. 금융공학보다는 투자금융(Investment Bank)분야에서 더 관심을 갖는 주제이다. 

전통적인 밸류에이션 모델은 종목 하나하나를 심층 분석하고 기업 활동이 주가에 영향을 예측하며 적정 주가를 찾는 반면, **금융공학은 종목 하나하나를 분석하기보다 유사한 다수 종목의 주가를 관찰해 주가를 형성하는 규칙을 찾아내고 이를 바탕으로 적정 주가를 찾는다.**

&nbsp;
&nbsp;
&nbsp;

# 주가 비교
주가 비교를 위해 삼성전자, SK 하이닉스 그리고 LG전자를 비교하였다. 

~~~python
pip install yfinance

import yfinance as yf
import matplotlib.pyplot as plt

samsung = yf.Ticker("005930.KS")samsung_hist = samsung.history(period="2y")

hynix = yf.Ticker("000660.KS")
hynix_hist = hynix.history(period="2y")

lg = yf.Ticker("066570.KS")
lg_history = lg.history(period="2y")
~~~

**삼성전자 주가**
~~~python
# plot samsung
plt.plot(samsung_hist.Close)
~~~

![Alt text](/public/post/2020_10_19_Stock_Index/Samsung.PNG)


**SK Hynix 주가**
~~~python
# plot Hynix
plt.plot(hynix_hist.Close)
~~~

![Alt text](/public/post/2020_10_19_Stock_Index/Hynix.PNG)

**LG 전자 주가**
~~~python
# plot Hynix
plt.plot(lg_history.Close)
~~~

![Alt text](/public/post/2020_10_19_Stock_Index/Lg.PNG)


**세 그래프 하나의 그래프로 합친 모습**

~~~python
plt.plot(samsung_hist.Close, color = 'Red')
plt.plot(hynix_hist.Close, color = 'Blue')
plt.plot(lg_history.Close, color = 'Green')
~~~

![Alt text](/public/post/2020_10_19_Stock_Index/combined.PNG)


어느 종목이 더 수익률이 높은 지 비교하려면 비교 대상들의 주가를 같은 기준으로 통일한 후 비교해야 한다. 이를 **지수화**라고 한다. 

**지수**는 비교 대상 가격을 기준일 가격으로 나누고 기준가를 곱해서 계산한다. 

**가격지수 = Price_t(대상일 주가) / Price_0(기준일 주가) x (기준가)


기준일을 2018년 10월 19일로 잡아보자. 그리고 기준가를 100으로 설정해보자. 

~~~python
# 삼성전자 가격지수 만들기 (2018년 10월 19일 날짜를 기준일 주가로 잡음)
samsung_hist['index_price'] = (samsung_hist['Close'] / samsung_hist['Close'].iloc[0]) * 100

# lg전자 가격지수 만들기 (2018년 10월 19일 날짜를 기준일 주가로 잡음)
lg_hist['index_price'] = (lg_hist['Close'] / lg_hist['Close'].iloc[0]) * 100

# 하아닉스 가격지수 만들기 (2018년 10월 19일 날짜를 기준일 주가로 잡음)
hynix_hist['index_price'] = (hynix_hist['Close'] / hynix_hist['Close'].iloc[0]) * 100

# 세 종목의 주가를 지수화 한 후 그래프로 비교
plt.plot(samsung_hist['index_price'], color = 'Red')
plt.plot(hynix_hist['index_price'], color = 'Blue')
plt.plot(lg_hist['index_price'], color = 'Green')
~~~

코로나 당시 LG전자의 하락폭이 가장 컸다. 하락 폭이 큰 만큼 다시 또 올라 2018년 대비 현재 140까지 LG전자는 상승했다. 

![Alt text](/public/post/2020_10_19_Stock_Index/combined_2.PNG)

이를 통해 가격대가 서로 다른 금융상품들은 지수화를 통해 비교 가능성을 높일 수 있다.

&nbsp;
&nbsp;
&nbsp;

# 지수란?
**지수(index)**란 구체적인 숫자 자체의 크기보다는 시간의 흐름에 따라 수량이나 가격 등 해당 수치가 어떻게 변화되었는지를 파악할 수 있도록 만든 것으로 통상 비교의 기준이 되는 시점(기준 시점)을 100으로 하여 산출. 

### 지수 산출 방법
지수를 산출하는 방법은 크게 구성종목 주가의 평균을 이용해 산출하는 **다우존스(Dow Jones)방식**과 구성종목 시가총액(주가 X 상장주식 수)의 합계를 이용해 산출하는 **시가총액 방식**의 두가지로 나뉜다. 


종목 | 주가 | 상장주식 수 | 시가총액 | 다우존스 방식 | 시가총액 방식
---|---|---|---|---|---
A | 1,000 | 1,000,000 | 1,000,000,000 | 1,000 | 1,000,000,000
B | 2,000 | 1,000,000 | 2,000,000,000 | 2,000 | 2,000,000,000
C | 1,000,000 | 1,000 | 1,000,000,000 | 1,000,000 | 1,000,000,000
D | 500 | 2,000,000 | 1,000,000,000 | 500 | 1,000,000,000
E | 1,000 | 1,000 | 1,000,000 | 1,000 | 1,000,000

&nbsp;

**다우존스 방식 : 200,900**

**시가총액 방식 : 5,001,000,000**

대부분의 지수는 시가총액 방식으로 산출하며 다우존스 방식은 잘 쓰지 않는 편이다. 요즈음 인기리에 개발되는 지수는 **시가총액 방식에 약간의 변형이 가해진 형태인데, 유동비율을 반영한 시가총액 방식이다.**

시가총액을 구할 때 (주가 x 상장주식 수)가 아닌 (주가 x 유동주식 수)로 구하는 것. 유동주식이란 실제로 거래소 시장에서 유통되는 주식을 말한다. 

&nbsp;
&nbsp;
&nbsp;

# 지수 vs 지수 : 회귀분석
규칙을 찾아내는 방법 중 가장 대표적인 예가 회귀분석인데, 여기에서는 S&P500과 KOSPI200 지수를 비교하며 규칙을 찾아보자. 

&nbsp;

### S&P500 vs. KOSPI200 지수 비교 1

&nbsp;

금융관련 기사, 애널리스트 레포트 등에서는 커플링(Coupling, 한 국가의 경제가 다른 국가의 경제에 큰 영향을 미쳐 유사하게 움직이는 현상), 디커플링이라는 용어로 국가 간 증시 흐름의 동조화 또는 탈동조화 현상을 표현한다. 


S&P500과 KOSPI의 2018년 10월 20일부터 2020년 10월 20일까지의 움직임을 한 번 보자. 

~~~python
# S&P 500 데이터 다운로드
sp_500 = yf.Ticker('^GSPC')
sp_500_hist = sp_500.history(period="2y")

# KOSPI 데이터 다운로드
kp = yf.Ticker('^KS11')
kp_hist = kp.history(period = '2y')

plt.plot(sp_500_hist.Close, color = 'Red')
plt.plot(kp_hist.Close, color = 'Blue')
~~~

두 지수의 스케일이 달라 움직임을 지수화하여 볼 필요가 있다. 

![Alt text](/public/post/2020_10_19_Stock_Index/sp_kp.PNG)

~~~python
# 지수화 화기
sp_500_hist['price_index'] = (sp_500_hist.Close / sp_500_hist.Close.iloc[0]) * 100
kp_hist['price_index'] = (kp_hist.Close / kp_hist.Close.iloc[0]) * 100

# 두 지수 한 그래프에 보여주기.
plt.plot(sp_500_hist.price_index, color = 'Red')
plt.plot(kp_hist.price_index, color = 'Blue')
~~~

S&P는 꾸준히 오르는 모습을 보이다가 코로나를 겪으면서 V자 반등 후 다시 오르고 있다. 

코스피의 경우 박스권에 갖혀있다가 코로나를 겪으면서 V자 반등 한 후 오르고 있는 추세이다. 

![Alt text](/public/post/2020_10_19_Stock_Index/sp_kp_2.PNG)

&nbsp;
&nbsp;
&nbsp;

두 나라 증시가 얼마나 유사하게 움직이는 지를 알아보기 위해 X축은 S&P500, Y축은 KOSPI로 이루어진 새로운 그래프를 그려보자. 이런 그래프를 **산포도**라고 하는데 X축과 Y축의 상관관계를 파악해 두 변수 간의 관련성을 파악하기 좋은 그래프 형식이다. 

~~~python
# 산포도 그리기

plt.scatter(sp_2017, kp_2017)
~~~

![Alt text](/public/post/2020_10_19_Stock_Index/scatter_1.PNG)

&nbsp;
&nbsp;
&nbsp;

## 회귀분석

**회귀분석**이란 둘 또는 그 이상의 변수 사이의 관계를 분석할 때 널리 쓰이는 통계적 방법. 결국 두 변수 사이의 관계를 나타내는 **추세선**을 찾아내는 것이 회귀분석의 목표이다. 추세선은 직선(y=ax+b)뿐 아니라 다항식 곡선(y=ax^3 + bx^2 + cx + d), 삼각함수(y=asinx + bcosx + c)등 필요에 따라 다양한 형태일 수 있다. 

~~~python
# 회귀분석 적용하기
from sklearn.linear_model import LinearRegression

line_fitter = LinearRegression()
line_fitter.fit(sp_201ㄴ7.values.reshape(-1,1), kp_2017.values.reshape(-1, 1))

# 회귀분석 : 두 변수 사이의 관계를 나타내는 **추세선** 을 찾아내는 것이 회귀분석의 
plt.plot(sp_2017, kp_2017, 'o')
plt.plot(sp_2017,line_fitter.predict(sp_2017.values.reshape(-1,1)))
plt.show()
~~~

우리가 찾고자 하는 것이 아래의 선이다. 

![Alt text](/public/post/2020_10_19_Stock_Index/regression_1.PNG)

직선 추세선의 공식은 중학교에서 배운 1차함수 y=ax+b이다. 여기서 a는 **기울기**, b는 **y절편**이라고 한다. 기울기가 1이면 정확히 x가 변하는 양만큼 y도 변한다는 뜻이고 1보다 크면 x가 변하는 것보다 y는 더 큰 폭으로 변화한다는 뜻이다. 

~~~python
# 기울기 그리고 절편 출력
print("기울기 : ", line_fitter.coef_[0][0])
print("절편 : ", line_fitter.intercept_[0])

print("회귀분석 : ", f"y = {line_fitter.coef_[0][0]}x + {line_fitter.intercept_[0]}")
~~~

기울기는 0.33이고 절편은 66이다. 즉, kospi가 변화하는 것보다 s&p가 변화하는 것이 더 크다는 의미이다. 

~~~
기울기 :  0.33404021376129106
절편 :  66.43068168216082
회귀분석 :  y = 0.33404021376129106x + 66.43068168216082
~~~

### S&P500 vs. KOSPI200 지수 비교 2

&nbsp;

앞에서는 최근 2년간의 지수를 비교했다면 이번에는 좀 더 긴 기간을 비교해보자. 2008년 리먼 브라더스발 금융 위기부터 2018년까지 기간이다. 

~~~python
sp = data['S&P500']
kp = data['KOSPI200']

sp_index = sp / sp.iloc[0] * 100
kp_index = kp / kp.iloc[0] * 100

# 두 지수 한 그래프에 보여주기.
plt.plot(sp_index, color = 'Red')
plt.plot(kp_index, color = 'Blue')
~~~

![Alt text](/public/post/2020_10_19_Stock_Index/sp_kP_3.PNG)

회귀분석을 적용해보자. 

~~~python
# 회귀분석 적용하기
from sklearn.linear_model import LinearRegression

line_fitter = LinearRegression()
line_fitter.fit(sp_index.values.reshape(-1,1), kp_index.values.reshape(-1, 1))

# 회귀분석 : 두 변수 사이의 관계를 나타내는 **추세선** 을 찾아내는 것이 회귀분석의 목표
plt.plot(sp_index, kp_index, 'o')
plt.plot(sp_index.values,line_fitter.predict(sp_index.values.reshape(-1,1)))
plt.show()
~~~

2016년 데이터에 비해 분포가 상당히 넓게 퍼져있고 직선의 모양과는 거리가 멀다. 

![Alt text](/public/post/2020_10_19_Stock_Index/regression_2.PNG)

기울기와 절편을 구해보자. 

기울기는 0.33, y절편은 66.43이다. 

~~~python
# 기울기 그리고 절편 출력
print("기울기 : ", line_fitter.coef_)
print("절편 : ", line_fitter.intercept_)

print("회귀분석 : ", f"y = {line_fitter.coef_[0][0]}x + {line_fitter.intercept_[0]}")

# 기울기 그리고 절편 출력
print("기울기 : ", line_fitter.coef_[0][0])
print("절편 : ", line_fitter.intercept_[0])

print("회귀분석 : ", f"y = {line_fitter.coef_[0][0]}x + {line_fitter.intercept_[0]}")
~~~

~~~
기울기 :  0.33404021376129106
절편 :  66.43068168216082
회귀분석 :  y = 0.33404021376129106x + 66.43068168216082
~~~

&nbsp;
&nbsp;
&nbsp;

### R^2 : 추세선의 품질
2개의 추세선 중 어느 추세선이 가장 품질이 좋을까? 이를 수치로 확인하는 방법이 **R^2**이다. 회귀분석을 하면 R^2값이 함께 출력되는데 이 값은 추세선의 품질을 나타내는 점수이다. 0부터 1사이 값이 나오는데, 1에 가까울 수록 상관관계가 높아진다. 이 R^2를 **결정계수**라고 한다. 


첫번째 케이스의 R^2 구하면 0.16이 나온다. 
~~~python
from sklearn.metrics import r2_score

# R2 구하기
y_test = kp_2017.values
y_predict = line_fitter.predict(sp_2017.values.reshape(-1,1))
print(r2_score(y_test, y_predict))
~~~

~~~
0.16015274451882489
~~~

두번쨰 케이스의 R^2를 구하면 0.52가 나온다. 
~~~python
from sklearn.metrics import r2_score

# R2 구하기
y_test = kp_index.values
y_predict = line_fitter.predict(sp_index.values.reshape(-1,1))
print(r2_score(y_test, y_predict))
~~~

~~~
0.5216558633947137
~~~

즉, 두번째 경우의 Linear Regression이 더 상관관계가 높다는 의미이다. 
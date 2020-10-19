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


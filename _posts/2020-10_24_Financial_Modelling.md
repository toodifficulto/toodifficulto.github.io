---
layout: post
title: 2. 크롤링 (파이썬을 활용한 금융공학 레시피)
tags:  [quant]
---

# S&P 500 vs. KOSPI 200비교
네이버와 다음에서 과거 주가 데이터를 수집(Scraping)해서 그래프를 그려보고(plotting) 회귀분석까지 해보자. 

&nbsp;
&nbsp;

### 네이버에서 KOSPI200 지수 수집하기

아래 사이트로 가자. 
https://finance.naver.com/sise/sise_index.nhn?code=KOSPI

코스피200 지수 정보가 나온다. 화면 중앙에는 KOSPI200 지수 그래프가 나오고 바로 아래는 최종거래일의 시간별 시세가, 그 아래에 일별 시세가 나온다. 

우리가 수집해야 할 정보는 **일별시세**이다. 
![Alt text](/public/post/2020_10_24_Finance_Crawling/chrome.PNG)

일별 시세의 웹페이지는 다음과 같다. 
https://finance.naver.com/sise/sise_index_day.nhn?code=KOSPI 

그리고 클릭해보면 아래와 같이 나온다. 

![Alt text](/public/post/2020_10_24_Finance_Crawling/day_price.PNG)

네이버에서는 page인수 뒤에 따라붙은 페이지 번호를 변경하는 방식으로 일별시세 정보가 움직인다. 즉 일별시세 URL은 

https://finance.naver.com/sise/sise_index_day.nhn?code=종목코드&page=페이지번호 이다.  

종목코드를 index_cd 변수에 저장하고 페이지 번호는 1부터 시작해보자. 

~~~python
index_cd = 'KPI200'
page_n = 2

naver_index = f'https://finance.naver.com/sise/sise_index_day.nhn?code={index_cd}&page={page_n}'

# 해당 URL 소스 코드를 긁어와서 source변수에 담아보자.
from urllib.request import urlopen
import bs4

source = urlopen(naver_index).read()

# 뷰티플수프를 이용해 태그별로 분류하자. 
source = bs4.BeautifulSoup(source, 'lxml')
print(source.prettify())
~~~

![Alt text](/public/post/2020_10_24_Finance_Crawling/source.PNG)

위에서 날짜와 가격을 가져오자.

~~~python
def historical_index_naver(index_cd, start_date='', end_date='', page_n=1, last_page=0):
    
    if start_date:   # start_date가 있으면
        start_date = date_format(start_date)   # date 포맷으로 변환
    else:    # 없으면
        start_date = dt.date.today()   # 오늘 날짜를 지정
    if end_date:   
        end_date = date_format(end_date)   
    else:   
        end_date = dt.date.today()  
        
        
    naver_index = 'http://finance.naver.com/sise/sise_index_day.nhn?code=' + index_cd + '&page=' + str(page_n)
    
    source = urlopen(naver_index).read()   # 지정한 페이지에서 코드 읽기
    source = bs4.BeautifulSoup(source, 'lxml')   # 뷰티풀 스프로 태그별로 코드 분류
    
    dates = source.find_all('td', class_='date')   # <td class="date">태그에서 날짜 수집   
    prices = source.find_all('td', class_='number_1')   # <td class="number_1">태그에서 지수 수집
    
    for n in range(len(dates)):
    
        if dates[n].text.split('.')[0].isdigit():
            
            # 날짜 처리
            this_date = dates[n].text
            this_date= date_format(this_date)
            
            if this_date <= end_date and this_date >= start_date:   
            # start_date와 end_date 사이에서 데이터 저장
                # 종가 처리
                this_close = prices[n*4].text   # prices 중 종가지수인 0,4,8,...번째 데이터 추출
                this_close = this_close.replace(',', '')
                this_close = float(this_close)

                # 딕셔너리에 저장
                historical_prices[this_date] = this_close
                
            elif this_date < start_date:   
            # start_date 이전이면 함수 종료
                return historical_prices              
            
    # 페이지 네비게이션
    if last_page == 0:
        last_page = source.find('td', class_='pgRR').find('a')['href']
        # 마지막페이지 주소 추출
        last_page = last_page.split('&')[1]   # & 뒤의 page=506 부분 추출
        last_page = last_page.split('=')[1]   # = 뒤의 페이지번호만 추출
        last_page = int(last_page)   # 숫자형 변수로 변환
        
    # 다음 페이지 호출
    if page_n < last_page:   
        page_n = page_n + 1   
        historical_index_naver(index_cd, start_date, end_date, page_n, last_page)   
        
    return historical_prices
~~~

이제 2018년 4월 1일부터 2018년 4월4일의 KPI200의 지수를 가져와보자. 

~~~python
index_cd = 'KPI200'
historical_prices = dict()

start = time.time()
historical_index_naver(index_cd, '2018-4-1', '2018-4-4')
print("총 걸린 시간 : ", time.time() - start)
historical_prices
~~~

~~~
총 걸린 시간 :  34.20585298538208
{datetime.date(2018, 4, 4): 308.54,
 datetime.date(2018, 4, 3): 313.38,
 datetime.date(2018, 4, 2): 314.0}
~~~

### 네이버에서 해외 지수 수집하기
네이버 해외주식의 경우 JSON형식의 데이터를 받아와서 정리한다. 

~~~python
def read_json(d, symbol, page=1):
    url = 'https://finance.naver.com/world/worldDayListJson.nhn?symbol='+symbol+'&fdtc=0&page='+str(page)
    raw = urlopen(url)
    data = json.load(raw)
    
    for n in range(len(data)):
        date = pd.to_datetime(data[n]['xymd']).date()
        price = float(data[n]['clos'])
        d[date] = price
        
    if len(data) == 10 and page<3:
        page += 1
        read_json(d, symbol, page)
        
    return (d)

def date_format(d=''):
    if d != '':
        this_date = pd.to_datetime(d).date()
    else:
        this_date = pd.Timestamp.today().date()   # 오늘 날짜를 지정
    return (this_date)

def index_global(d, symbol, start_date='', end_date='', page=1):

    end_date = date_format(end_date)
    if start_date == '':
        start_date = end_date - pd.DateOffset(months=1)
    start_date = date_format(start_date)
        
    url = 'https://finance.naver.com/world/worldDayListJson.nhn?symbol='+symbol+'&fdtc=0&page='+str(page)
    raw = urlopen(url)
    data = json.load(raw)
    
    if len(data) > 0:
        
        for n in range(len(data)):
            date = pd.to_datetime(data[n]['xymd']).date()

            if date <= end_date and date >= start_date:   
            # start_date와 end_date 사이에서 데이터 저장
                # 종가 처리
                price = float(data[n]['clos'])
                # 딕셔너리에 저장
                d[date] = price
            elif date < start_date:   
            # start_date 이전이면 함수 종료
                return (d)              

        if len(data) == 10:
            page += 1
            index_global(d, symbol, start_date, end_date, page)
        
    return (d)
~~~

&nbsp;
&nbsp;

# 데이터프레임으로 여러 딕셔너리를 테이블 하나로 합치기
이제 S&P500과 KOSPI200 지수의 2017년 1월 1일부터 2017년 12월 31일까지의 데이터를 뽑아서 각각 sp500, kospi200이라는 딕셔너리에 저장하고 데이터 프레임을 만들어보자. 

~~~python
index_cd = 'KPI200'
historical_prices = dict()
kospi200 = historical_index_naver(index_cd, '2017-1-1', '2017-12-31')

index_cd = 'SPI@SPX'
historical_prices = dict()
sp500 = index_global(historical_prices, index_cd, '2017-1-1', '2017-12-31')

# kospi200과 sp500 딕셔너리를 df라는 dataframe에 넣는다
tmp = {'S&P500' : sp500, 'KOSPI200' : kospi200}
df = pd.DataFrame(tmp)
~~~

![Alt text](/public/post/2020_10_24_Finance_Crawling/df.PNG)

보면 2017년 1월 2일 S&P500의 데이터가 없다. 아무래도 1월 2일부터 1월 16일은 미국 증권시장 휴장일이라서 그렇다. 이렇게 빠진 데이터를 채우는 것이 수학에서는 **보간**이라고 한다. 데이터프레임은 보간을 위한 몇 가지 방법을 제공하는데, 금융 데이터 처리를 위해 여기에서 사용하는 보간을 위한 방법은 **DataFrame.fillna(method='ffill') 또는 DataFrame.fillna(method='bfill')입니다.** ffill은 'forward fill', 즉 앞의 데이터로 뒤의 구멍을 채우라는 뜻이고, bfill은 'backward fill', 즉 뒤의 데이터로 앞 구멍을 채우라는 뜻이다. 

~~~python
df = df.fillna(method='ffill')
if df.isnull().values.any():
    df = df.fillna(method='bfill')
    
df
~~~

![Alt text](/public/post/2020_10_24_Finance_Crawling/df_2.PNG)

이제 2008년부터 2017년까지 데이터를 가져와보자. 

~~~python
index_cd = 'KPI200'
historical_prices = dict()
kospi200 = historical_index_naver(index_cd, '2008-1-1', '2017-12-31')

index_cd = 'SPI@SPX'
historical_prices = dict()
sp500 = index_global(historical_prices, index_cd, '2008-1-1', '2017-12-31')

# kospi200과 sp500 딕셔너리를 df라는 dataframe에 넣는다
tmp_2 = {'S&P500' : sp500, 'KOSPI200' : lkospi200}
df_2 = pd.DataFrame(tmp_2)

df_2 = df_2.fillna(method = 'ffill')
if df_2.isnull().values.any():
    df_2 = df.fillna(method='bfill')
~~~

&nbsp;
&nbsp;
&nbsp;

### 맷플롯립을 이용해 그래프 그리기
이번에는 앞에서 만든 S&P500과 KOSPI200 지수 데이터를 가지고 그래프를 그려보자.

~~~python
import matplotlib.pyplot as plt
%matplotlib inline

plt.figure(figsize=(10,5)) # 크기 조절
plt.plot(df_2['S&P500']) # 데이터 선택
plt.plot(df_2['KOSPI200'])
plt.legend(['S&P500', 'KOSPI200']) # 범례 위치 지정
plt.grid(True, color='0.7', linestyle=':', linewidth=1) # 그리드 설정
~~~

![Alt text](/public/post/2020_10_24_Finance_Crawling/graph.PNG)

비교 가능성을 위해 다시 한 번 지수화 작업을 해준다. 

~~~python
plt.figure(figsize=(10,5)) # 크기 조절
plt.plot(df_2['S&P500'] / df_2['S&P500'].iloc[-1] * 100) # 데이터 선택
plt.plot(df_2['KOSPI200'] / df_2['KOSPI200'].iloc[-1] * 100)
plt.legend(['S&P500', 'KOSPI200']) # 범례 위치 지정
plt.grid(True, color='0.7', linestyle=':', linewidth=1) # 그리드 설정
~~~

![Alt text](/public/post/2020_10_24_Finance_Crawling/graph_2.PNG)

&nbsp;
&nbsp;
&nbsp;

# 회귀분석
이번에는 회귀분석을 이용해 두 지수의 상관관계를 분석해보자. 먼저 2016 1월 1일부터의 데이터를 활용해보자. 

~~~python
df_2_2016_ratio = df_2.loc[dt.date(2016, 1, 4): ] / df_2.loc[dt.date(2016, 1, 4)] * 100

plt.figure(figsize=(5,5))
plt.scatter(df_2_2016_ratio['S&P500'], df_2_2016_ratio['KOSPI200'], marker='.')
plt.grid(True, color='0.7', linestyle=':', linewidth=1)
plt.xlabel('S&P500')
plt.ylabel('KOSPI200')
~~~

![Alt text](/public/post/2020_10_24_Finance_Crawling/scatter.PNG)

회귀분석을 위해서는 sklearn.linear_model내의 LinearRegression 라이브러리를 사용한다. 벡터화된 데이터 처리를 위해 **넘파이(NumPy)** 모듈도 함꼐 사용된다.

1) 우선 numpy와 LinearRegression 라이브러리를 호출한다.

2) S&P500 지수가 KOSPI200에 미치는 영향을 보려는 분석이므로, 즉 KOSPI200이 주가 되는 분석이기 때문에 X값인 독립변수(independent variable)에는 S&P500 지수를, Y값인 종속변수(dependent variable)에는 KOSPI200 지수를 넣는다. 

3) **LinearReggression** 모듈은 n행 1열 벡터를 입력받기 때문에 지수 데이터를 (n,1) 모양으로 변경해야 한다. Array.reshape(-1, 1)로 벡터 형태를 바꿀 수 있다. 

4) LinearRegression의 fit명령어로 회귀분석을 시행한다.

5) 회뷔분석 결과값인 기울기는 'Slope', y절편은 'Intercept', R^2값은 'R^2'라는 키에 저장한 딕셔너리를 반환한다. 

~~~python
import numpy as np
from sklearn.linear_model import LinearRegression

x = df_2_2016_ratio['S&P500']
y = df_2_2016_ratio['KOSPI200']

# 1개 칼럼 np.array로 변환
independent_var = np.array(x).reshape(-1, 1)
dependent_var = np.array(y).reshape(-1, 1)

# Linear Regression
regr = LinearRegression()
regr.fit(independent_var, dependent_var)

result = {"Slope" : regr.coef_[0,0], 'Intercept' : regr.intercept_[0], 'R^2' : regr.score(independent_var, dependent_var)}
result
~~~

결과에 따르면 기울기는 0.46, 절편은 67 그리고 R^2는 0.37이 나온다. 
~~~
{'Slope': 0.46484650712502523,
 'Intercept': 67.2955823409034,
 'R^2': 0.3763681759209512}
 ~~~

 이 추세선을 그래프로 그려보자.
 ~~~PYTHON
 plt.figure(figsize=(5,5))
plt.scatter(independent_var, dependent_var, marker='.', color='skyblue')
plt.plot(independent_var, regr.predict(independent_var), color='r', linewidth=3)
plt.grid(True, color='0.7', linestyle=':', linewidth=1)
plt.xlabel('S&P500')
plt.ylabel('KOSPI200')
~~~

![Alt text](/public/post/2020_10_24_Finance_Crawling/graph_3.PNG)

&nbsp;
&nbsp;
&nbsp;
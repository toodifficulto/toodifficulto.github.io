---
layout: post
title: 다음 주식 크롤링
tags:  [crawling]
---
[다음 금융 사이트](http://finance.daum.net/)에서 아래와 같은 인기 검색어을 크롤링 하는 실습을 해보자.

![Alt text](/public/post/2020_03_29_daum/rank_pic.PNG)

먼저 이전까지 했던 방식으로 다음 금융 사이트를 스크래핑 해보자.

~~~python
import urllib.request as req

url = 'http://finance.daum.net/'

res = req.urlopen(url)

print(res.read().decode('UTF-8'))
~~~

그러면 아래와 같은 결과값이 나오는데 이 중 우리가 원하는 인기검색어에 대한 내용은 없다.

~~~
<!DOCTYPE html>
<html lang="ko">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<meta http-equiv="X-UA-Compatible" content="IE=edge" />
<link rel="shortcut icon" href="//finance.daum.net/favicon.ico" type="image/x-icon" />

<title>Daum 금융  </title>

<link rel="stylesheet" type="text/css" href="/dist/common.css?v=1584080256" />
<link rel="stylesheet" type="text/css" href="/dist/custom.ui.css?v=1584080256" />

<script>
    window.REQUEST_URI = '/home';
    window.CURRENT_URL = encodeURIComponent(
        "".concat(
            window.location.protocol,
            "//",
            window.location.host,
            window.location.pathname,
            window.location.search
        )
    );
    window.FINANCE = {};
    window.FINANCE.BASE_URL = '/dist';
    window.FINANCE.VERSION = '1584080256';
    window.FINANCE.API_URL = '';
    window.FINANCE.HOST = '//finance.daum.net';
    window._REALTIME_HOST = 'wss://realtime.finance.daum.net/websocket';
    window._WISE_FN_HOST = '//wisefn.finance.daum.net/v1';
    window._PERIOD = '';
</script>


<script type="text/javascript" src="/dist/requirejs/require.js?v=1584080256"></script>

<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-128578811-1"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'UA-128578811-1');
</script>

<!--[if lt IE 9]> <script type="text/javascript" src="/dist/html5.js?v=1584080256"></script> <![endif]-->

<link rel="stylesheet" type="text/css" href="/dist/layerpopup.css?v=1584080256" />
</head>
<body class="finance">
    <div id="wrap">
        <div class="header">
    <div class="head_media f_clear">
        <h1><a href="//finance.daum.net" id="kakaoServiceLogo" class="#gnb #default #service_finance"><span class="ir_wa">금융</span></a></h1>
        <strong class="screen_out">관련서비스</strong>
        <ul class="kakaoRelServices">
            <li><a href="http://realestate.daum.net"><span class="ir_wa">부동산</span></a></li>
        </ul>
        <h2 class="screen_out">검색</h2>
        <div class="search_news" id="boxSearch"></div>


<script>
    requirejs(["/dist/daum/app/config.js?v=1584080256"], function (common) {
        require(["app/common/search"]);
    });
</script>


    </div>
    <div id="kakaoGnb" role="navigation">
        <div class="inner_gnb">
            <h2 class="screen_out">금융 메인메뉴</h2>
            <ul class="gnb_comm">
                <li class="on">
                    <a href="/" class="link_gnb link_gnb1">
                        <span class="screen_out">선택됨</span>
                        <span class="ir_wa">홈</span>
                        <span class="bar_gnb"><span class="inner_bar"></span></span>
                    </a>
                </li>
                <li class="">
                    <a href="/domestic" class="link_gnb link_gnb2">
                        <span class="ir_wa">국내</span>
                        <span class="bar_gnb"><span class="inner_bar"></span></span>
                    </a>
                </li>
                <li class="">
                    <a href="/global" class="link_gnb link_gnb3">
                        <span class="ir_wa">해외</span>
                        <span class="bar_gnb"><span class="inner_bar"></span></span>
                    </a>
                </li>
                <li class="">
                    <a href="/news" class="link_gnb link_gnb4">
                        <span class="ir_wa">뉴스</span>
                        <span class="bar_gnb"><span class="inner_bar"></span></span>
                    </a>
                </li>
                <li class="">
                    <a href="/investment" class="link_gnb link_gnb5">
                        <span class="ir_wa">투자정보</span>
                        <span class="bar_gnb"><span class="inner_bar"></span></span>
                    </a>
                </li>
                <li class="">
                    <a href="/talks" class="link_gnb link_gnb6">
                        <span class="ir_wa">종목토론</span>
                        <span class="bar_gnb"><span class="inner_bar"></span></span>
                    </a>
                </li>
                <li class="">
                    <a href="/my" class="link_gnb link_gnb8">
                        <span class="ir_wa">MY</span>
                        <span class="bar_gnb"><span class="inner_bar"></span></span>
                    </a>
                </li>
            </ul>
            <div class="gnb_etc">
                <ul class="gnb_with">
                    <li class="">
                        <a href="/exchanges" class="link_gnb link_gnb1">
                            <span class="ir_wa">환율</span>
                            <span class="bar_gnb"><span class="inner_bar"></span></span>
                        </a>
                    </li>
                    <li>
                        <a href="http://insurance.dunamu.com" class="link_gnb link_gnb3">
                            <span class="ir_wa">보험</span>
                            <span class="bar_gnb"><span class="inner_bar"></span></span>
                        </a>
                    </li>
                </ul>
                <div class="gnb_banner" id="boxKakaoStockBanner">
                    <a href="javascript:void(0)" class="btnKakaoStockBanner"><span class="ir_wa">쓰던 증권사 그대로, 카카오스탁</span></a>
                </div>
            </div>
        </div>
    </div>
</div>
<div id="notice"></div>

<script>
    requirejs(["/dist/daum/app/config.js?v=1584080256"], function (common) {
        require(["app/common/notice"]);
    });
</script>


<script>
    requirejs(["/dist/daum/app/config.js?v=1584080256"], function (common) {
        require(["app/common/kakaostock"]);
    });
</script>



        <div class="stockB" id="boxTodayIndexes">
    <div class="center">
        <div class="stock" id="boxIndexes"></div>
        <div class="stockTab" id="boxIndexTabs">
            <span>
                <h4>오늘의 증시</h4>
                <p id="labTodayDate"></p>
                <p id="labGlobalTodayDate">현지시간 기준</p>
            </span>
            <ul>
                <li><a href="javascript:void(0)" data-type="DOMESTIC" class="on" title="국내 지수">국내 지수</a></li>
                <li><a href="javascript:void(0)" data-type="USA" title="미국 지수">미국 지수</a></li>
                <li><a href="javascript:void(0)" data-type="ASIA" title="아시아 지수">아시아 지수</a></li>
                <li><a href="javascript:void(0)" data-type="EUROPE" title="유럽 지수">유럽 지수</a></li>
            </ul>
        </div>
    </div>
</div>
<div class="mainB">
    <div class="leftW">
        <div class="topNews" id="boxTodayNews"></div>
        <div class="rankingB" id="boxBestSearchs"></div>
        <div class="rankingB line" id="boxTopSectors">
        </div>
    </div>
    <div class="rightW" id="boxRightSidebar">
        <div class="btnB">
            <a href="javascript:void(0)" title="전 종목 시세 보기" id="btnAllQuotes"><i>-</i>전 종목 시세 보기</a>
        </div>
        <div class="myB" id="boxMy"></div>
        <div class="sponsor"></div>
    </div>
</div>
<div class="investmentB">
    <div class="center" id="boxColumns">
    </div>
</div>
<div class="mainB last">
    <div class="leftW max">
        <div class="rankingB" id="boxMarketTrend"></div>
    </div>
</div>

<script>
    requirejs(["/dist/daum/app/config.js?v=1584080256"], function (common) {
        require(["app/home/index"]);
    });
</script>



        <div class="footer">
    <span>
        <div class="fl">
            <ul>
                <li><a target='_blank' href="http://policy.daum.net/info/info" title="서비스 약관/정책">서비스 약관/정책</a></li>    
                <li><a target='_blank' href="http://cs.daum.net/faq/19.html" title="금융 고객센터">금융 고객센터</a></li>
                <li><a target='_blank' href="https://cs.daum.net/question/19.html" title="금융 문의하기">금융 문의하기</a></li>      
                <li><a target='_blank' href="http://blog.daum.net/daumfinance/" title="공지사항">공지사항</a></li>
            </ul>
            <p>카카오가 제공하는 증권정보는 단순히 정보의 제공을 목적으로 하고 있으며, 사이트에서 제공되는 정보는 오류 및 지연이<br>
발생될 수 있습니다. 제공된 정보이용에 따르는 책임은 이용자 본인에게 있으며, 카카오는 이용자의 투자결과에 따른<br>법적 책임을 지지 않
습니다.</p>
        </div>
        <div class="fr">
            <p>위 내용에 대한 저작권 및 법적 책임은 자료제공사 또는<br>글쓴이에 있으며 카카오의 입장과 다를 수 있습니다.</p>
            <p>Copyright (C) Kakao Corp. All rights reserved.</p>
        </div>
    </span>
</div>


        <div id="kakaoWrap">
    <div id="wrapMinidaum"></div>
</div>

<script>
    requirejs(["/dist/daum/app/config.js?v=1584080256"], function (common) {
        require(["app/common/mini_daum"]);
    });
</script>

    </div>
</body>
</html>
~~~~

나오지 않는 이유는 인기검색어는 __'Ajax를 통해 비동기적으로 데이터를 불러오기 때문'__ 이다.

먼저, 아래 개발자 도구의 __Network 탭을__ 보자. Network 탭은 daum Server에서 우리의 브라우저로 데이터를 송신하는 것을 보여준다.
![Alt text](/public/post/2020_03_29_daum/network_tabl.PNG)

그리고 제일 상단 위에 있는 __finance.daum.net__ 은 처음 우리가 req로 보낸 요청과 같다.
__finance.daum.net__ 을 살펴보면 Request 방식은 GET 방식이고 Status code는 200이다.

![Alt text](/public/post/2020_03_29_daum/fiance_daum_1.PNG)

그리고 __response__ 에 들어가서 살펴보면 처음에 우리가 req로 보낸 값과 같은 것을 알 수 있다.

![Alt text](/public/post/2020_03_29_daum/response.PNG)

즉 우리가 원하는 인기검색어는 __ajax를 통해 비동기식으로 데이터를 송신 받기 때문에 finance.daum.net에 없다__

이럴 경우 두 가지 방식이 존재한다.

__1. Selenium을 이용해 headless browser로 접근하여 동적 데이터를 로드시켜 가져오는 것__

__2. 직접 필요 데이터에 접근하여 정적인 데이터를 가져오는 것이었다.__

이번엔 직접 필요데이터에 접근해서 데이터를 가지고 와보자.

아래 __Network Tab__ 을 잘 살펴보면 __ranks?limit=10__ 이 있는 걸 볼 수 있다.

![Alt text](/public/post/2020_03_29_daum/ranks_10.PNG)

이 GET request가 바로 우리가 원하는 인기검색어 10개를 반환해주는 response이다.

![Alt text](/public/post/2020_03_29_daum/response_1.PNG)

&nbsp;
&nbsp;
&nbsp;

# Fake-UserAgent 사용하기.
하지만 http://finance.daum.net/api/search/ranks?limit=10 를 클릭해보면 다음과 같이 forbidden 페이지가 뜬다.이는 다음 사이트에서 website를 통해 클릭한 것과 우리처럼 python을 통해 scraping하는 것을 구분해 막기 때문이다.

![Alt text](/public/post/2020_03_29_daum/forbidden.PNG)

그래서 우리는 아래처럼 __User-Agent__ 와 __Referer__ 를 함께 서버에 보내주어야 한다.

![Alt text](/public/post/2020_03_29_daum/useragent.PNG)

그래서 이번에 __Fake-UserAgent__ 를 사용해서 보내보자.

먼저 FakeUserAgent를 설치하자.

~~~
pip install FakeUserAgent
~~~

그런 다음 import를 해주고 사용해보자. ua라는 객체를 만들어서 ua.chrome과 ua.ie를 출력하면 현재 chrome과 internet explorer의 userAgent 정보값이 출력된다.

~~~python
from fake_useragent import UserAgent

ua = UserAgent()

print(ua.ie)
print(ua.chrome)
print(ua.random)
~~~

~~~
Mozilla/5.0 (compatible, MSIE 11, Windows NT 6.3; Trident/7.0;  rv:11.0) like Gecko
Mozilla/5.0 (Windows NT 4.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2049.0 Safari/537.36
Mozilla/5.0 (Windows NT 6.2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/28.0.1467.0 Safari/537.36
~~~

그런 다음 headers값을 주어서 request를 보낸다.

~~~python
from fake_useragent import UserAgent
import urllib.request as req

ua = UserAgent()

print(ua.ie)
print(ua.chrome)
print(ua.random)

headers = {
    'User-agent' : ua.chrome,
    'referer' : ' http://finance.daum.net/'
}

url = 'http://finance.daum.net/api/search/ranks?limit=10'

# 요청
res = req.urlopen(req.Request(url, headers = headers )).read().decode('UTF-8')

print(res)
~~~

그러면 아래와 같이 response가 json형태로 돌아온다.

~~~
{"data":[{"rank":1,"rankChange":0,"symbolCode":"A096530","code":"KR7096530001","name":"씨젠","tradePrice":115900.0,"change":"RISE","changePrice":1400.0,"changeRate":0.0122270742,"chartSlideImage":null,"isNew":false},{"rank":2,"rankChange":0,"symbolCode":"A005930","code":"KR7005930003","name":"삼성전자","tradePrice":48300.0,"change":"RISE","changePrice":500.0,"changeRate":0.010460251,"chartSlideImage":null,"isNew":false},{"rank":3,"rankChange":0,"symbolCode":"A033270","code":"KR7033270000","name":"유나이티드제약","tradePrice":20150.0,"change":"UPPER_LIMIT","changePrice":4650.0,"changeRate":0.3,"chartSlideImage":null,"isNew":false},{"rank":4,"rankChange":0,"symbolCode":"A005690","code":"KR7005690003","name":"파미셀","tradePrice":19000.0,"change":"RISE","changePrice":3600.0,"changeRate":0.2337662338,"chartSlideImage":null,"isNew":false},{"rank":5,"rankChange":0,"symbolCode":"A068270","code":"KR7068270008","name":"셀트리
온","tradePrice":184000.0,"change":"RISE","changePrice":2500.0,"changeRate":0.0137741047,"chartSlideImage":null,"isNew":false},{"rank":6,"rankChange":0,"symbolCode":"A245620","code":"KR7245620000","name":"EDGC","tradePrice":14550.0,"change":"UPPER_LIMIT","changePrice":3350.0,"changeRate":0.2991071429,"chartSlideImage":null,"isNew":false},{"rank":7,"rankChange":0,"symbolCode":"A180640","code":"KR7180640005","name":"한진칼","tradePrice":57200.0,"change":"UPPER_LIMIT","changePrice":13150.0,"changeRate":0.2985244041,"chartSlideImage":null,"isNew":false},{"rank":8,"rankChange":2,"symbolCode":"A253840","code":"KR7253840003","name":"수젠텍","tradePrice":29350.0,"change":"RISE","changePrice":1750.0,"changeRate":0.0634057971,"chartSlideImage":null,"isNew":false},{"rank":9,"rankChange":0,"symbolCode":"A000660","code":"KR7000660001","name":"SK하이닉스","tradePrice":83300.0,"change":"RISE","changePrice":2600.0,"changeRate":0.0322180917,"chartSlideImage":null,"isNew":false},{"rank":10,"rankChange":-2,"symbolCode":"A059090","code":"KR7059090001","name":"미코","tradePrice":12500.0,"change":"UPPER_LIMIT","changePrice":2860.0,"changeRate":0.2966804979,"chartSlideImage":null,"isNew":false}]}  
~~~

그러면 json을 불러내서 우리가 볼 수 있는 형태로 출력한다.
~~~python
import json
rank_json = json.loads(res)['data']


for elm in rank_json:
#   print(type(elm), elm)
    print('순위 : {}, 금액 : {}, 회사명 : {}'.format(elm['rank'], elm.get('tradePrice') , elm['name']))
~~~

~~~
순위 : 1, 금액 : 115900.0, 회사명 : 씨젠
순위 : 2, 금액 : 48300.0, 회사명 : 삼성전자
순위 : 3, 금액 : 20150.0, 회사명 : 유나이티드제약
순위 : 4, 금액 : 19000.0, 회사명 : 파미셀
순위 : 5, 금액 : 184000.0, 회사명 : 셀트리온
순위 : 6, 금액 : 14550.0, 회사명 : EDGC
순위 : 7, 금액 : 57200.0, 회사명 : 한진칼
순위 : 8, 금액 : 29350.0, 회사명 : 수젠텍
순위 : 9, 금액 : 83300.0, 회사명 : SK하이닉스
순위 : 10, 금액 : 12500.0, 회사명 : 미코
~~~

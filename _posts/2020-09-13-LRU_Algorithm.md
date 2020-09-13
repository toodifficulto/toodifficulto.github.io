---
layout: post
title:  LRU 알고리즘
tags:  [algorithm-and-data-structure]
---

# LRU 알고리즘 (Least Recently Used Algorithm)

[참조1](https://j2wooooo.tistory.com/121)

[참조2](https://gomguard.tistory.com/115)

LRU 알고리즘은 **가장 오랫동안 참조되지 않은 페이지를 교체하는 기법**이다. 

&nbsp;
&nbsp;
&nbsp;

# 페이지 교체 알고리즘

컴퓨터는 보통 주기억 장치인 **램**과 보조기억 장치인 **하드나 ssd등의 대용랑 기억장치**를 가지고 있다. 

램이 속도가 빠르기 때문에 **보조기억장치로부터 데이터를 램에 저장해놓고 램에 있는 데이터를 가지고 빠르게 연산한다.** 이 때 램을 같은 크기의 블록으로 구성해서 운용하는데 **이 블록을 페이지라고** 부른다. 

![Alt text](/public/post/2020_09_13_LRU_Algorithm/pic1.png)

만약 cpu가 계산을 할 때 **필요한 데이터가 페이지에 있다면 cache hit** 이라고 부르며, **없을 경우에는 보조기억장치로부터 데이터를 페이지로 옮겨온 후에 계산을 하는데 이를 cache miss**라고 한다. 

**hit**일 때와 **miss**일 때 시간이 차이가 많이 나기 때문에 빠른 연산을 하기 위해선 **어떤 정보를 오래 저장해놓아야 하는 지가 매우 중요한 문제**이다. 

페이지 교체 알고리즘에는 대표적으로 다음과 같다. 

**FIFO(First In First Out)**

**OPT(Optimal Page Replacement)**

**LRU(Least Recently Used)**

**Count-Based**

**LFU(Least Frequently Used)**

**MFU(Most Frequently Used)**

&nbsp;
&nbsp;
&nbsp;

# LRU 알고리즘 

* LRU는 가장 최근에 사용되지 않은 것을 제거하는 알고리즘이다. 기본 가설은 **가장 오랫동안 사용하지 않았던 데이터는 앞으로도 사용할 확률이 적다는 것**이다. 

* RAM(Cache)에 존재하는 페이지 중 **가장 오랫동안 있던 페이지**를 교체한다. 

* 만약 RAM에 **존재하는 페이지가 참조될 경우 참조된 페이지는 가장 최근에 사용된 것으로 작동한다.** 

예를 들어, 다음과 같이 cache 크기가 3인 RAM이 있다고 하자. 

![Alt text](/public/post/2020_09_13_LRU_Algorithm/pic2.png)

참조값이 0,1,2 일때는 cache size가 꽉 차지 않았고 cache에 (0,1,2)와 동일한 페이지가 존재하지 않았으므로 **page miss**가 발생하고 cache에 저장된다. 

그 다음 참조값인 3의 경우 cache에 존재하지 않는다. 그러므로 **가장 오래된 페이지와 교체해야 한다.** 이럴 경우 **0**이 가장 오래 있었으므로 0과 3을 교체한다. 

다음 참조값인 4와 1도 동일하게 각각 가장 오래된 페이지인 1과 2와 교체된다. 

다음 참조값인 3의 경우 현재 cache에 (3,4,1)에 존재한다. 이럴 경우 **3은 가장 최근에 들어온 참조된 페이지로 변환되어 다른 페이지가 교체된다.**

그래서 다음 참조값인 2의 경우 3이 가장 오래되었지만 대신 다음으로 오래된 4가 2와 교체된다. 

&nbsp;
&nbsp;
&nbsp;

# LRU 파이썬 구현
아래는 프로그래머스의 LV2 문제 **캐시**에서 사용된 코드이다. 크게 흐름만 체크하면 된다. HIT일 경우 1, MISS일 경우 5의 시간이 든다. cities를 위의 참조값으로 생각하면 된다. 

cache는 stack구조로 되어 있다. 그래서 가장 오래된 page는 가장 아래에 존재한다. 

그리고 cities에 있는 city가 돌면서 만약 cache에 존재하면 걸린 시간은 HIT만큼 더해주고 해당 city를 가장 최근 페이지로 만들어 주기 위해 cache에서 city를 빼주고 위에다가 다시 넣는다. 

만약 city가 cache에 존재하지 않는 다면 cache에 넣어주는 데 만약 cache가 꽉찼다면 cache에 가장 오래된 페이지와 교체한다. cache가 꽉차지 않았다면 cache의 가장 위에 넣어준다. 

~~~python
cities = [city.lower() for city in cities]
    
    hit = 1
    miss = 5
    
    cache = []
    time = 0
    
    for city in cities:
        if city in cache:
            time += hit
            cache.remove(city)
            cache.append(city)
            
        else:
            if len(cache) < cacheSize:
                time += miss
                cache.append(city)
            else:
                cache.append(city)
                cache.pop(0)
                time += miss
            
    return time
~~~

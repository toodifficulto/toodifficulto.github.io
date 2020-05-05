---
layout: post
title: 파이썬과 비동기 프로그래밍
tags: [python]
---

# 비동기(Asynchronous)란?
예를 들어 10대의 세탁기와 10대의 커피포트에 물을 끓인다고 하자.

**첫번째 방법**
* 1번 세탁기를 돌린다.
* 1번 세탁기가 완료될 때까지 기다린다.
* 1번 세탁기에서 빨래를 수거한다.
* 2번 세탁기를 돌린다.
* 2번 세탁기가 완료될 때까지 기다린다.
... (위의 과정 반복)
* 2번 세탁기에서 빨래를 수거한다.
* 10번 세탁기를 돌린다.
* 10번 세탁기가 완료될 때까지 기다린다.
* 10번 세탁기에서 빨래를 수거한다.
* 1번 커피포트에 물을 끓인다.
* 1번 커피포트의 물로 녹차를 탄다.
* 1번 커피포트의 물이 끓기를 기다린다.
* 2번 커피포트의 물을 끓인다.
* 2번 커피포트의 물이 끓기를 기다린다.
* 2번 커피포트의 물로 녹차를 탄다.
... (위의 과정 반복)
* 10번 커피포트의 물을 끓인다.
* 10번 커피포트의 물이 끓기를 기다린다.
* 10번 커피포트의 물로 녹차를 탄다.

**두번째 방법**
* 1번 세탁기를 돌린다.
* 1번 세탁기가 완료되길 기다리지 않고 2번 세탁기를 돌린다.
... (위의 과정 반복)
* 9번 세탁기가 완료되길 기다리지 않고 10번 세탁기를 돌린다.
* 10번 세탁기가 완료되길 기다리지 않고 1번 커피포트에 물을 끓인다.
* 1번 커피포트의 물이 끓길 기다리지 않고 2번 커피포트에 물을 끓인다.
... (위의 과정 반복)
* 10번 커피포트에 물을 끓인다.
* 각 작업이 완료되는 순서대로 처리한다. (빨래 수거/녹차 타기)

두번째 방식으로 일을 처리하는 것이 **비동기적 처리** 라 하고, 첫 번째 방식으로 일을 처리하는 것을 **동기적 처리** 라고 한다.

비동기적 처리 방식과 동기적 처리 방식의 가장 큰 차이는 **기다리는 시간의 존재 여부이다.** 비동기적 처리방식에는 기다리는 시간에 바로 다른 작업을 하는 반면 동기적 처리 방식에선 하나의 작업을 시작했으면 그 작업이 끝날 때 까지 다른 작업을 실행하지 않는다.

프로그래밍에서도 응답을 기다려야 하는 일이 발생 했을 때 기다리지 않고 바로 다른 작업을 하는 것을 **비동기적 프로그래밍** , 응답이 올 때까지 기다린 이후에 다른 작업을 하는 것을 **동기적 프로그래밍** 이라고 한다.


&nbsp;
&nbsp;
&nbsp;

# 멀티쓰레드 vs 비동기
멀티쓰레드는 말 그대로 여러 개의 쓰레드가 동시에 parallel하게 실행이 되는 것이다. 하지만, **파이썬에서는 GIL(Global Interpreter Lock)때문에 제대로 실행되지 않는다.**



#### Global Interpreter Lock이란
파이썬 인터프리터가 한 쓰레드만 하나의 바이트코드를 실행할 수 있게 하는 것이다. 즉, GIL은 하나의 쓰레드에게 모든 자원의 점유를 허락한다.

&nbsp;
&nbsp;

# 파이썬 비동기
파이썬에서 비동기 프로그래밍을 하기 위해선 **이벤트 루프** 와 **코루틴** 을 이해해야 한다.

#### 1. 이벤트 루프(Event Loop)
이벤트 루프는 작업들을 루프(반복문)를 돌면서 **하나씩** 실행시키는 역할을 한다. 이때 만약 실행된 작업이 특정한 데이터를 요청하고 응답을 기다려야 한다면, 이 작업은 다시 이벤트 루프에 통제권을 넘겨준다. 통제권을 받은 이벤트 루프는 다음 작업을 실행한다. 그리고 응답을 받은 순서대로 **멈췄던 부분부터** 다시 통제권을 가지고 작업을 마무리한다.

#### 2. 코루틴(Coroutine)
파이썬에서는 이러한 작업들은 **코루틴(Coroutine)** 으로 이루어져 있다. **코루틴은 응답이 지연되는 부분에서 이벤트 루프에 통제권을 줄 수 있으며, 응답이 완료되었을 때 멈추었던 부분에서 기존의 상태를 유지한 채 남은 작업을 완료할 수 있는 함수** 를 의미한다.

파이썬에서 코루틴이 아닌 일반적인 함수를 **서브루틴(Subroutine)** 이라고 한다.

![Alt text](/public/post/2020_05_01_python_asyncio/pic2.jpg)

&nbsp;
&nbsp;
&nbsp;

# 기초
파이썬에서 비동기를 사용하기 위해서 asyncio 모듈을 이용한다. 함수 앞에 asyncio를 붙이면 코루틴을 만들 수 있다. 또한 병목이 발생해서 다른 작업으로 통제권을 넘겨줄 필요가 있는 부분에는 await을 써준다. 이때 await뒤에 오는 함수도 코루틴으로 작성되어야 한다. asyncio.sleep 함수는 코루틴으로 구현되어 있기 때문에 비동기로 동작한다. (time.sleep은 사용할 수 없다.)

코루틴으로 태스크를 만들었다면 asyncio.get_event_loop 함수를 활용해 이벤트 루프를 정의하고 run_until_complete로 실행시킬 수 있다.

~~~python
import asyncio
import time

async def sleep():
    await asyncio.sleep(5)

start = time.time()

loop = asyncio.get_event_loop()

loop.run_until_complete(sleep())

end = time.time()

print(str(end - start) + 's')
~~~

~~~
5.004921197891235s
~~~

&nbsp;
&nbsp;
&nbsp;

# 동기적 처리 & 비동기적 처리
**동기적 처리**
~~~python
import time

def sub_routine_1():
    print("서브루틴 1 시작")
    print("서브루틴 1 중단 ... 5초간 대기")
    time.sleep(5)
    print("서브루틴 1 종료")

def sub_routine_2():
    print("서브루틴 2 시작")
    print("서브루틴 2 중단 ... 5초간 대기")
    time.sleep(5)
    print("서브루틴 2 종료")

if __name__ == "__main__":

    sub_routines = [sub_routine_1, sub_routine_2]

    start = time.time()

    for i in range(2):
        sub_routines[i]()

    end = time.time()

    print("time taken : {}".format(end - start))
~~~

~~~
time taken : 10.001477241516113
~~~

**비동기적 처리**
비동기로 두 개 이상의 작업(코루틴)을 돌릴 때에는 asyncio.gather 함수를 이용한다. 이때, 각 태스크들은 unpacked 형태로 넣어주어야 한다. 즉, asyncio.gather(coroutine_1(), coroutine_2())처럼 넣어주거나 asyncio.gather(\*[coroutine_1(), coroutine_2()])처럼 넣어주저야 한다.

~~~python
import asyncio
import time

async def coroutine_1(): # 코루틴 정의 async를 앞에 붙여준다.
    print("코루틴 1 시작")
    print("코루틴 1 중단 .. 5초간 대기")
    # await으로 중단점 설정(블락킹되는 부분에서 사용)
    await asyncio.sleep(5)
    print("코루틴 1 재개")

async def coroutine_2(): # 코루틴 정의 async를 앞에 붙여준다.
    print("코루틴 2 시작")
    print("코루틴 2 중단 .. 4초간 대기")
    # await으로 중단점 설정(블락킹되는 부분에서 사용)
    await asyncio.sleep(4)
    print("코루틴 2 재개")

if __name__ == "__main__":
    # 이벤트 루프 정의
    loop = asyncio.get_event_loop()

    start = time.time()

    # 두 개의 코루틴을 이벤트 루프에서 도린다.
    # 코루틴이 여러개 일 경우, asyncio.gather을 먼저 이용(순서대로 스케쥴링 된다.)
    loop.run_until_complete(asyncio.gather(coroutine_1(), coroutine_2()))

    end = time.time()

    print('time taken : {}'.format(end - start))
~~~

~~~
time taken : 5.004921197891235
~~~

동기적으로 처리했을 때는 약 10초 걸렸던 작업이 비동기적으로 처리했을 땐 거의 5초만에 처리할 수 있었다. 또한 비동기적 처리에서 **코루틴1** 을 먼저 실행했지만 **코루틴2** 의 대기가 먼저 끝나자 **코루틴 2** 를 먼저 처리하고 **코루틴 1** 을 처리하는 것을 확인 할 수 있었다.

&nbsp;
&nbsp;
&nbsp;

# 코루틴으로 짜여있지 않은 함수 비동기적으로 이용하기

위에서 await 뒤에 오는 함수 역시 코루틴으로 작성되어 있어야 비동기적으로 작업을 할 수 있다고 하였다. 하지만 파이썬 대부분의 라이브러리는 비동기를 고려하지 않았다. 그래서 이벤트 루프의 **run_in_executor** 를 사용하면 가능하다.

**asyncio.get_event_loop()** 를 활용해서 현재 이벤트 루프를 loop라는 이름으로 받아오고 loop.run_run_in_executor 를 사용한다. **첫 번 째 인자** 로 concurrent.futures.Executor 의 객체가 들어가고 (None을 써주면 asyncio의 내장 executor가 들어간다), **두 번 째 인자** 로는 사용하고자 하는 함수, **그 이후의 인자(*args)** 에는 사용하고자 하는 함수의 인자들을 써주면 된다.

~~~python
import asyncio
import time

async def coroutine_1():
    print("코루틴 1 시작")
    print("코루틴 1 중단 .. 5초간 기다린다")
    loop = asyncio.get_event_loop()

    # run_in_executor: 코루틴으로 짜여져 있지 않은 함수(서브루틴)을
    # 코루틴처럼 실행시켜주는 메소드

    # Params of run_in_executor:
    # executor(None: default loop executor), func, *args
    # 또는 concurrent.futures.Executor의 인스턴스 사용가능

    await loop.run_in_executor(None, time.sleep, 5)

    print("코루틴 1 재개")

async def coroutine_2():
    print("코루틴 2 시작")
    print("코루틴 2 중단 .. 4초간 기다린다")
    loop = asyncio.get_event_loop()

    # run_in_executor: 코루틴으로 짜여져 있지 않은 함수(서브루틴)을
    # 코루틴처럼 실행시켜주는 메소드

    # Params of run_in_executor:
    # executor(None: default loop executor), func, *args
    # 또는 concurrent.futures.Executor의 인스턴스 사용가능

    await loop.run_in_executor(None, time.sleep, 4)

    print("코루틴 2 재개")

if __name__ == '__main__':

    loop = asyncio.get_event_loop()

    start = time.time()

    loop.run_until_complete(asyncio.gather(coroutine_1(), coroutine_2()))

    end = time.time()

    print("time taken : {}".format(end - start))
~~~

~~~
time taken : 5.011038303375244
~~~

asyncio.sleep 을 사용한 것과 유사한 결과를 볼 수 있다. 사실 이건 비동기적 처리로 보이지만 실제로 **쓰레딩** 을 이용한 것이라고 할 수 있다.

하지만 쓰레딩을 이용하는 것도 비용이 만만치 않다. 파이썬에서는 **GIL** 때문에 쓰레드들이 동시에 작업이 불가능하므로 다른 쓰레드를 호출하는데 걸리는 시간을 낭비하게 된다.(Context Switching의 비용)

&nbsp;
&nbsp;
&nbsp;

# run_in_executor (쓰레딩)를 활용한 비동기

~~~python
import asyncio
import time
from concurrent.futures import ProcessPoolExecutor, ThreadPoolExecutor


async def sleep(executor=None):
    loop = asyncio.get_event_loop()
    await loop.run_in_executor(executor, time.sleep, 1)


async def main():

    # max_workers에 따라서 실행시간이 달라지는 것을 확인할 수 있다.
    # (하지만 workers가 많아질수록 컨텍스트 스위칭 비용도 커진다.)
    # None으로 하는 경우는 디폴트로 설정한 workers수가 작아서 인지 훨씬 더 오래걸린다.

    executor = ThreadPoolExecutor(max_workers=10000)

    # asyncio.ensure_future함수는 태스크를 현재 실행하지 않고,
    # 이벤트 루프가 실행될 때 실행할 것을 보증해주는 함수
    futures = [
        asyncio.ensure_future(sleep(executor)) for i in range(10000)
    ]
    await asyncio.gather(*futures)


if __name__ == "__main__":
    start = time.time()
    # python 3.7부터는 이벤트 루프를 따로 명시적으로 지정하지 않고,
    # asyncio.run으로 돌릴 수 있다.
    asyncio.run(main())
    end = time.time()
    print(f'time taken: {end-start}')
~~~

~~~
>> time taken: 3.2648983001708984
~~~

컨텍스트 비용이 약 2초가량 발생했다.

# asyncio.sleep을 활용한 비동기
~~~python
import asyncio
import time

async def sleep():
    await asyncio.sleep(1)


async def main():
    # asyncio.sleep은 아무리 많아져도 비동기적으로 잘 돌아간다.
    futures = [
        asyncio.ensure_future(sleep()) for i in range(10000)
    ]
    await asyncio.gather(*futures)


if __name__ == "__main__":
    start = time.time()
    asyncio.run(main())
    end = time.time()
    print(f'{end-start}')
~~~

~~~
>> 1.1979601383209229
~~~

asyncio.sleep 은 컨텍스트 스위칭에 대한 비용이 발생하지 않는다.


# 출저
https://sjquant.tistory.com/14?category=797018

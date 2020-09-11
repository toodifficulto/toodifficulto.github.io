---
layout: post
title:  Programmers [3차] 방금그곡
tags:  [algorithm-problem]
---

## [https://programmers.co.kr/learn/courses/30/lessons/17683](https://programmers.co.kr/learn/courses/30/lessons/17683)

# 문제 
방금그곡
라디오를 자주 듣는 네오는 라디오에서 방금 나왔던 음악이 무슨 음악인지 궁금해질 때가 많다. 그럴 때 네오는 다음 포털의 '방금그곡' 서비스를 이용하곤 한다. 방금그곡에서는 TV, 라디오 등에서 나온 음악에 관해 제목 등의 정보를 제공하는 서비스이다.

네오는 자신이 기억한 멜로디를 가지고 방금그곡을 이용해 음악을 찾는다. 그런데 라디오 방송에서는 한 음악을 반복해서 재생할 때도 있어서 네오가 기억하고 있는 멜로디는 음악 끝부분과 처음 부분이 이어서 재생된 멜로디일 수도 있다. 반대로, 한 음악을 중간에 끊을 경우 원본 음악에는 네오가 기억한 멜로디가 들어있다 해도 그 곡이 네오가 들은 곡이 아닐 수도 있다. 그렇기 때문에 네오는 기억한 멜로디를 재생 시간과 제공된 악보를 직접 보면서 비교하려고 한다. 다음과 같은 가정을 할 때 네오가 찾으려는 음악의 제목을 구하여라.

* 방금그곡 서비스에서는 음악 제목, 재생이 시작되고 끝난 시각, 악보를 제공한다.

* 네오가 기억한 멜로디와 악보에 사용되는 음은 C, C#, D, D#, E, F, F#, G, G#, A, A#, B 12개이다.

* 각 음은 1분에 1개씩 재생된다. 음악은 반드시 처음부터 재생되며 음악 길이보다 재생된 시간이 길 때는 음악이 끊김 없이 처음부터 반복해서 재생된다. 음악 길이보다 재생된 시간이 짧을 때는 처음부터 재생 시간만큼만 재생된다.

* 음악이 00:00를 넘겨서까지 재생되는 일은 없다.

* 조건이 일치하는 음악이 여러 개일 때에는 라디오에서 재생된 시간이 제일 긴 음악 제목을 반환한다. 재생된 시간도 같을 경우 먼저 입력된 음악 제목을 반환한다.

* 조건이 일치하는 음악이 없을 때에는 `(None)`을 반환한다.

**입력 형식**

입력으로 네오가 기억한 멜로디를 담은 문자열 m과 방송된 곡의 정보를 담고 있는 배열 musicinfos가 주어진다.

* m은 음 1개 이상 1439개 이하로 구성되어 있다.

* musicinfos는 100개 이하의 곡 정보를 담고 있는 배열로, 각각의 곡 정보는 음악이 시작한 시각, 끝난 시각, 음악 제목, 악보 정보가 ','로 구분된 문자열이다.

    * 음악의 시작 시각과 끝난 시각은 24시간 HH:MM 형식이다.
    
    * 음악 제목은 ',' 이외의 출력 가능한 문자로 표현된 길이 1 이상 64 이하의 문자열이다.
    
    * 악보 정보는 음 1개 이상 1439개 이하로 구성되어 있다.

**출력 형식**

조건과 일치하는 음악 제목을 출력한다.

# 문제풀이
두 번의 정규표현식으로 sort를 하면 된다.

첫번째 정규표현식(**^(\D*)**)은 숫자가 나오기 전까지 문자열을 빼는 표현식이다. ^는 시작을 뜻하며 \D는 숫자가 아닌 문자열을 뜻한다. 즉, 숫자가 아닌 문자로 시작하는 문자열을 구하는 표현식이다. 

두번째 정규표현식은 숫자만 빼는 표현식이다. \d는 숫자를 의미한다. 그러므로 \d+는 숫자만 가져오는 표현식이다. 

**입출력예시**
m | musicinfos | answer
---|---|---
ABCDEFG | [12:00,12:14,HELLO,CDEFGAB, 13:00,13:05,WORLD,ABCDEF] | HELLO
CC#BCC#BCC#BCC#B | [03:00,03:30,FOO,CC#B, 04:00,04:08,BAR,CC#BCC#BCC#B] | FOO
ABC | [12:00,12:14,HELLO,C#DEFGAB, 13:00,13:05,WORLD,ABCDEF] | WORLD

&nbsp;

**설명**

첫 번째 예시에서 HELLO는 길이가 7분이지만 12:00부터 12:14까지 재생되었으므로 실제로 CDEFGABCDEFGAB로 재생되었고, 이 중에 기억한 멜로디인 ABCDEFG가 들어있다.

세 번째 예시에서 HELLO는 C#DEFGABC#DEFGAB로, WORLD는 ABCDE로 재생되었다. HELLO 안에 있는 ABC#은 기억한 멜로디인 ABC와 일치하지 않고, WORLD 안에 있는 ABC가 기억한 멜로디와 일치한다.

&nbsp;
&nbsp;
&nbsp;

# 문제풀이
주어진 musicinfos에 있은 음악 정보들을 ','기준으로 나눈다. 그리고 시작시간과 끝시간 사이 시간을 분으로 구한다. 

#이 들어간 음계는 나중에 구분하기 쉽게 소문자로 바꾼다. 

예를 들어, C#DEFGAB가 있다면 cDEFGAB로 바꾼다. 그리고 구한 시간 차 동안 음계를 이어 붙인다.

그런 다음, 주어진 m이 음계에 있고 시간 차가 긴 순서대로 answer에 name을 저장한다.

&nbsp;
&nbsp;
&nbsp;

# 코드
~~~python
from datetime import datetime

def solution(m, musicinfos):
    
    answer = ''
    found = False
    longest_min = 0
    
    replace_dict = {"C#" : "c", "D#" : "d", "F#" : "f", "G#" : "g", "A#" : 'a'}
    
    for musics in musicinfos:
        start, end, name, music = musics.split(',')
        tdelta = datetime.strptime(end, "%H:%M") - datetime.strptime(start, "%H:%M")
        minutes = tdelta.seconds // 60
                
        for key in replace_dict.keys():
            if key in music:
                music = music.replace(key, replace_dict[key])
            if key in m:
                m = m.replace(key, replace_dict[key])
                
        temp = ""
        
        for i in range(minutes):
            temp += music[i % len(music)]

        music = temp

     
        if m in music:
            if minutes > longest_min:
                answer = name
                found = True
                longest_min = minutes

                
    if not found:
        answer = '(None)'
    
    return answer
~~~
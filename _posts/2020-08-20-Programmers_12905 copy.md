---
layout: post
title:  Programmers 가장 큰 정사각형 찾기
tags:  [algorithm-problem]
--- 

## [https://programmers.co.kr/learn/courses/30/lessons/12905](https://programmers.co.kr/learn/courses/30/lessons/12905)

# 문제 
1와 0로 채워진 표(board)가 있습니다. 표 1칸은 1 x 1 의 정사각형으로 이루어져 있습니다. 표에서 1로 이루어진 가장 큰 정사각형을 찾아 넓이를 return 하는 solution 함수를 완성해 주세요. (단, 정사각형이란 축에 평행한 정사각형을 말합니다.)

예를 들어

![Alt text](/public/post/2020_08_20_12905/pic1.png)

가 있다면 가장 큰 정사각형은

![Alt text](/public/post/2020_08_20_12905/pic2.png)


가 되며 넓이는 9가 되므로 9를 반환해 주면 됩니다.

&nbsp;
&nbsp;

**제한사항**

* 표(board)는 2차원 배열로 주어집니다.

* 표(board)의 행(row)의 크기 : 1,000 이하의 자연수

* 표(board)의 열(column)의 크기 : 1,000 이하의 자연수

* 표(board)의 값은 1또는 0으로만 이루어져 있습니다.

&nbsp;

### **입출력 예**
board | answer
--- | --- | ---
[[0,1,1,1],[1,1,1,1],[1,1,1,1],[0,0,1,0]] | 	9
[[0,0,1,1],[1,1,1,1]] | 4

&nbsp;
&nbsp;
&nbsp;

# 문제풀이
Dynamic Programming을 사용하여 문제를 풀었다. 


아래처럼 1로 이루어진 네 칸이 기본단위이다. 그래서, 네 칸이 다 1 이상인 경우, 가장 큰 정사각형의 최소 길이는 2가 된다.
![Alt text](/public/post/2020_08_20_12905/pic3.png)

그러므로, 아래처럼 가장 오른쪽 아래의 칸을 제외한 세 칸중에서 가장 작은 숫자에 1을 더한 값을 가장 오른쪽 아래의 칸에 넣는다.

![Alt text](/public/post/2020_08_20_12905/pic4.png)

위와 같은 로직으로 돌아가다보면, 가장 큰 길이인 3이 나오게 된다.

![Alt text](/public/post/2020_08_20_12905/pic5.png)


&nbsp;
&nbsp;
&nbsp;

# 코드
**for문 사용한 코드**
~~~python
def solution(board):
    
    height = len(board)
    width = len(board[0])
    
    if width < 2 or height < 2:
        return 1

    max_length = 0
    max_area = 0

    for i in range(1, height):
        for j in range(1, width):
            if board[i][j] >= 1 and board[i][j-1] >= 1 and board[i-1][j] >=1 and board[i-1][j-1] >=1:
                min_num = min(board[i][j-1], board[i-1][j], board[i-1][j-1])

                board[i][j] = min_num + 1

        if max(board[i]) > max_length:
            max_length = max(board[i])
            max_area = max_length ** 2
    
    return max_area
~~~

&nbsp;
&nbsp;
&nbsp;

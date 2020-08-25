---
layout: post
title:  Programmers 올바른 괄호
tags:  [algorithm-problem]
--- 

## [https://programmers.co.kr/learn/courses/30/lessons/12909](https://programmers.co.kr/learn/courses/30/lessons/12909)

# 문제 
괄호가 바르게 짝지어졌다는 것은 '(' 문자로 열렸으면 반드시 짝지어서 ')' 문자로 닫혀야 한다는 뜻입니다. 예를 들어

()() 또는 (())() 는 올바른 괄호입니다.
)()( 또는 (()( 는 올바르지 않은 괄호입니다.
'(' 또는 ')' 로만 이루어진 문자열 s가 주어졌을 때, 문자열 s가 올바른 괄호이면 true를 return 하고, 올바르지 않은 괄호이면 false를 return 하는 solution 함수를 완성해 주세요.
&nbsp;
&nbsp;

**제한사항**
* 문자열 s의 길이 : 100,000 이하의 자연수

* 문자열 s는 '(' 또는 ')' 로만 이루어져 있습니다.

&nbsp;

### **입출력 예**
s | answer
---|---
()() | true
(())() | true
)()( | false
(()( | false

&nbsp;
&nbsp;
&nbsp;

# 문제풀이
괄호가 바르게 짝지어져 있다는 것은 "("가 나타나면 무조건 ")"가 나와야 한다는 것이다. 그래서 **stack**을 활용하였다. 

"("가 나타나면 stack에 넣는다. 그리고 ")"이 나타나면 stack에서 "("를 pop한다. 만약 "("와 ")"의 개수의 짝이 맞추어져 있지 않으면 stack이 비어있는데 pop을 해서 error가 난다거나, 모든 순서가 끝나고 stack이 비어있지 않고 "("가 들어가 있다. 

예를 들어, "()()"가 stack에 들어간다면,

1. "("는 stack에 "("이 들어간다. ["("]

2. ")"는 stack에서 "("을 뺀다. []

3. "("는 stack에 "("를 넣는다. ["("]

4. ")"는 stack에서 "("를 뺀다. []

결과가 끝나고, stack은 비어있으므로 "()"의 괄호는 짝지어져 있다는 얘기다.

반대로, "(()("가 stack에 들어간다면, 

1. "("는 stack에 "("를 넣는다. ["("]

2. "("는 stack에 "("를 넣는다. ["(", "("]

3. ")"는 stack에서 "("를 뺀다. ["("]

4. "("는 stack에 "("를 넣는다. ["(", "("]

결과가 끝나고, stack은 비어있지 않다. 그러므로, "()"의 괄호는 짝지어져 있지 않다는 얘기다.

&nbsp;
&nbsp;
&nbsp;

# 코드
def solution(s):
    answer = True
    
    stack = []
    
    try:
        for ch in s:
            if ch == '(':
                stack.append(ch)
            elif ch == ')':
                stack.pop()
    except:
        return False
    
    if len(stack) != 0:
        return False
    
    return True
~~~
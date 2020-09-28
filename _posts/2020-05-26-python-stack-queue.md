---
title: 파이썬 스택과 큐!
excerpt: 파이썬에서 스택과 큐는 어떻게 쓰는 걸까?
categories:
  - python
tags:
  - python
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2020-05-26T13:00-18:00
permalink: python-stack-queue.html
---
## 파이썬 스택과 큐

파이썬으로 스택과 큐 예제를 풀려고 하는데 파이썬 스택과 큐 라이브러리가 없는것 같다... ~~그럼 직접구현해야하나??~~  파이썬에는 ```c++```이나 ```java```와 같이 스택과 큐 라이브러리가 없다. 그래서 list 자료형을 응용해서 하는 방법과 ```collections``` 라이브러리의 ```deque```를 이용한다.

- list 자료형 이용하기
- collections의 deque

## list 자료형으로 스택과 큐 구현

스택 구현

```python
#스택
stack = []
# push함수로 append 사용
stack.append('a')
stack.append('b')
stack.append('c')
# pop
stack.pop() # -> 'c'
stack.pop() # -> 'b'
stack.pop() # -> 'a'
```

큐 구현

```python
# 큐
queue = []
# push 함수는 스택과 마찬가지로~
queue.append('a')
queue.append('b')
queue.append('c')
# pop
queue.pop(0) # -> 'a' 
queue.pop(0) # -> 'b'
queue.pop(0) # -> 'c'
```

스택과 큐 모두 위와 같이 구현된다. 정말 단순하고 쉽다...

큐는 FIFO이기 때문에 pop을 할 땐 0번째 인덱스로 pop을 하면 FIFO를 할 수 있다.

그럼 ```empty```는 어떻게 처리해야 되나??

단순하다. 그냥 리스트자체를 조건에 넣으면 된다.

```python
#스택
stack = []
stack.append('a')
stack.append('b')
stack.append('c')

while stack:
    stack.pop()

    
# 'c', 'b', 'a'    
```

위 처럼 리스트가 ```[]```이면 False이고 리스트에 element가 들어 있으면 True이기 때문이다.

## collections deque

deque~~(디큐?? 데크??)~~는 **double-ended queue** 의 줄임말이다. 양방향으로 데이터를 처리할 수 있는 자료구조를 의미한다. 그러니까 앞으로도 넣고 빼고, 뒤로도 넣고 빼고 할 수 있다.~~(마치 강장동물같다.)~~ 

deque의 함수는 list와 함수차이만 있다.

```python
from collections import deque

deq = deque()

# 1. append()
# 걍 list와 같음. 뒤로 데이터 추가한다는 소리.
deq.append(1)
deq.append(2)
deq.append(3)
# deq : [1,2,3]
# 2. appendleft()
# append는 뒤로 넣고 appendleft는 앞으로 넣음. 이럴거면 append도 appendright라 하지.
deq.appendleft(2)
deq.appendleft(3)
# deq : [3,2,1,2,3]
# append가 있으면 pop도 있듯이 appendleft가 있으니까 popleft도 있다.
# 예제 생략!
f
# 3.rotate()
# deque안의 요소를 변수값 만큼 회전 시킴.
deq.rotate(1)
# deq: [3,3,2,1,2] 참고로 양수면 오른쪽.
deq.rotate(-2)
# deq: [2,1,2,3,3] 음수면 왼쪽~.
```

python의 새로운 collections을 하나 배웠다. stack/queue 문제를 풀어보면서 감을 익혀야겠다!
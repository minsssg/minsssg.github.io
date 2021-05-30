---
title: "ArrayList vs LinkedList vs Vector"
excerpt: "ArrayList, LinkedList, vector의 차이를 공부해보자"
author_profile: true
categories:
  - JAVA
tags:
  - JAVA
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2021-05-30T16:00-17:00
header:
  teaser: "/assets/images/java-teaser.jpg"
---

# 1. List란 무엇인가

리스트(List)는 요소(element)를 순서가 있는 것을 나열한 것이다. 리스트를 이야기할 때, 집합(Set)과 비교하여 이야기할 수 있다. 집합(Set)은 유일하고 순서가 없는(unordered) 요소들을 모아놓은 것을 말한다.

다음 그림에서 자바 Collection 다이어그램을 보여준다.

![java-collection-list](/assets\images\java-study\list\java-collection-list.PNG)

# 2. ArrayList vs LinkedList vs Vector

위 사진의 계층형 다이어그램을 보면 `ArrayList`, `LinkedList`, `Vector` 는 `List`인터페이스의 구현체인 것을 알 수 있다. 따라서 `ArrayList`, `LinkedList`, `Vector` 의 쓰임새는 비슷하며, 구현방식의 차이로 인해 다른 연산을 위해 다른 성능을 가진다.

## ArrayList

`ArrayList`는 변경가능한 배열(Array)을 구현한 것이다. `ArrayList`는 동적으로 크기가 변경된다. `ArrayList`는 기본적으로  배열(Array)이기 때문에 `get(), set()`를 통해 요소(element)에  바로(directly) 접근할 수 있다.

## LinkedList

`LinkedList`는 더블 링크드 리스트로 구현되었다. 더블 링크드 리스트는 다음 그림처럼 양방향으로 연결된 리스트를 말하며, 양쪽으로 접근이 가능한 것을 말한다.

![double-linked-list](/assets\images\java-study\list\double-linked-list.png)

`LinkedList`는 `ArrayList`보다 요소 추가(add), 삭제(remove)에서 더 좋은 성능을 보이지만, `get(), set()`메서드는 좋지 못하다.

## Vector

`Vector`는 `ArrayList`와 비슷하지만, 동기화가 된다.  이 때문에, `Vector`는 `ArrayList`보다 `overhead`가 크다. 보통 많은 자바 프로그래머들은 `Vector`보단 `ArrayList`를 사용하는데,  자바 프로그램에서 명시적으로 동기화 할 수 있기 때문이다.

`thread-safe` 프로그램에선 `ArrayList`를 쓰는 것이 좋다. 

`Vector`는 새로운 공간이 필요할 경우, 배열의 사이즈를 2배씩 증가시키지만, `ArrayList`는 절반씩 사이즈를 증가시킨다.

# 3. Performance of ArrayList vs LinkedList

시간 복잡도 비교표

|          | ArrayList |     LinkedList     |
| -------- | :-------: | :----------------: |
| get()    |   O(1)    |        O(n)        |
| add()    |   O(1)    | O(1) **amortized** |
| remove() |   O(n)    |        O(n)        |

* `ArrayList`의 임의적인 `add/remove` 연산은 *O(n)*을 갖지만, 리스트의 마지막일 경우, *O(1)*을 깆는다.
* `LinkedList`도 임의적인 `add/remove`의 경우 *O(n)* 성능을 갖지만, 리스트의 처음과 마지막의 경우, *O(1)*을 갖는다.

`LinkedList`는 다음의 경우에 사용하는 것이 좋다.

* 요소의 임의의 엑세스 수가 많지 않을 때.
* add/remove 연산이 많을 때.

# 참고자료

* [https://www.programcreek.com/2013/03/arraylist-vs-linkedlist-vs-vector/](https://www.programcreek.com/2013/03/arraylist-vs-linkedlist-vs-vector/)
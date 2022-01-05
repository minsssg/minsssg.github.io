---
title: Java8 Features
excerpt: "Java8 추가된 점 정리"
author_profile: true
categories:
  - JAVA
tags:
  - JAVA
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2021-09-02T21:00-22:00
header:
  teaser: "/assets/images/java-teaser.jpg"
---

# 1. Java8의 달라진 점☕

* Interface Default and Static Methods
* Method reference
* Optional
* Stream API

# 2. Interface Default and Static Methods

Java8 이전의 `interface`는 `public`접근자의 추상메서드로 작성되었다. 

`interface`로 구현된 클래스는 인터페이스에 정의된 모든 메서드를 구현해서 사용해야 했다.

java8 부터는 인터페이스에 `static`, `default` 메서드로 작성할 수 있게 되었다.  즉, 인터페이스 내에서 메서드를 정의할 수 있게 되었다.

## 2.1. Static Method

```java
public interface Vehicle {
    
    static String producer() {
        return "Tesla";
    }
}
```

위 처럼 `producer()`란 메서드를 `interface`내에서 정의하고 있다. 이제 `producer()`를 따로 재정의(Override)하지 않고 사용할 수 있다.

```java
String producer = Vehicle.producer();
```

## 2.2. Default Method



# 3. Method References

매서드 참조는 람다식에서 메서드를 호출하는 대신 짧고 읽기 쉬운 대안으로 사용된다.

## 3.1. Reference to a Static Method

`Static Method` 참조 문법은 다음과 같다. `Static Method`참조는 람다식에서 사용가능하다.

`ContainingClass::methodName`



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


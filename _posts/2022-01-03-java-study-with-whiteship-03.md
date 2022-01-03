---
title: 자바스터디 3주차
excerpt: "3주차 과제: 자바 연산자"
author_profile: true
categories:
  - JAVA
tags:
  - JAVA
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2022-01-03T21:00-22:00
header:
  teaser: "/assets/images/java-teaser.jpg"
---

# 개요🙌

**목표:** 자바가 제공하는 다양한 연산자를 학습하자.

**학습할 것**

* 산술 연산자(Arithmetic Operators)
* 비트 연산자(Bitwise and Bit shift operators)
* 관계 연산자
* 논리 연산자
* instanceof
* assignment(=) operator
* 화살표(->) 연산자
* 3항 연산자
* 연산자 우선 순위
* (선택사항) Java 13, switch 연산자

# 산술 연산자(Arithmetic Operator)

 산술연산자는 **사칙연산**을 의미한다.

산술연산자는 기본적으로 두 개의 피연산자(operand)를 요한다. 

| Operator | Description                                                  |
| -------- | ------------------------------------------------------------ |
| +        | 더하기 연산자(`String`연산의 경우, 문자열 붙이기로 사용된다.) |
| -        | 빼기 연산자(`String`포함 안됨.)                              |
| *        | 곱하기 연산자                                                |
| /        | 나누기 연산자, 몫이 나옴.                                    |
| %        | 나머지 연산자, 나머지 값이 나옴.                             |

# 비트 연산자

비트 연산자는 비트를 다루는 연산자이다.

| Operator                | Description                                                  |
| ----------------------- | ------------------------------------------------------------ |
| ~[operand]              | 보수 연산자, 0 단위, 1단위의 비트를 변경함.                  |
| [operand1]<<[operand2]  | [operand1]의 비트들을 [operand2]만큼 왼쪽으로 이동함.        |
| [operand1]>>[operand2]  | [operand1]의 비트들을 [operand2]만큼 오른쪽으로 이동함. 부호에 맞게 가장 왼쪽값을 채운다.(-일 경우, 1로 채움) |
| [operand1]>>>[operand2] | >> 행위는 동일하나, LMB(Left Most Bit)에 0으로 채운다.       |
| &                       | AND 연산자                                                   |
| ^                       | XOR 연산자                                                   |
| \|                      | OR 연산자                                                    |

# 관계 연산자

관계 연산자는 하나의 피연산자가 다른 피연산자보다 크거나 작거나 같거나 같지 않음을 비교하는 연산자이다.

| Operator                 | Description                                    |
| ------------------------ | ---------------------------------------------- |
| [operand1] == [operand2] | 같은지 비교                                    |
| [operand1] != [operand2] | 다른지 비교                                    |
| [operand1] > [operand2]  | [operand1]이 [operand2]보다 더 큰지 비교       |
| [operand1] >= [operand2] | [operand1]이 [operand2]보다 같거나 큰지 비교   |
| [operand1] < [operand2]  | [operand1] 이 [operand2]보다 더 작은지 비교    |
| [operand1] <= [operand2] | [operand1]가 [operand2]보다 같거나 작은지 비교 |

**주의:** "=<", "=>" 이런 연산자는 없음. 헷갈림 주의

# 논리 연산자

조건이 참인지 거짓인지 판별하는 연산자

| Operator                     | Description                                              |
| ---------------------------- | -------------------------------------------------------- |
| [condition1]&&[condition2]   | [condition1]과(AND) [condition2]가 둘 다 TRUE면 TRUE     |
| [condition1]\|\|[condiiton2] | [condition1] 또는(OR) [condition2] 중 하나만 TRUE면 TRUE |

**short-circuiting**: 논리 연산자에서 "&&" 연산의 경우, 둘 다 참이여야 "참"인데, 처음 [condition1]이 `false`라면 [condition2]은 보지 않아도 되고, "\|\|"의 연산의 경우, [condition1]이 참이면 뒤의 [condition2]은 보지 않아도 `true`가 된다. 이렇게 연산을 줄이는 것을 `short-circuit`연산이라고 한다.



# 단일 연산자

단일연산자(unary Operator)는 피연산자(Operand)가 하나만 있을 때, 사용하는 연산자를 말한다.

| Operator | Description                                                  |
| -------- | ------------------------------------------------------------ |
| +        | 양수(positive) 나타낼 때, 사용한다. (하지만 대부분 생략하고 나타낸다.) |
| -        | 단일 마이너스 연산자는 음수(negative)를 나타낸다.            |
| ++       | 1 증가 단일 연산자                                           |
| --       | 1 감소 단일 연산자                                           |
| !        | 논리 보수 연산자로, Not을 의미한다. boolean값일 때 사용.     |

# instanceof

`instanceof` 연산자는 객체의 특정 타입을 비교하는 연산자이다.

생성된 객체가 어떤 `interface`, `class`의  객체인지 판별하는 연산자이다.

**주의:** `null`은 어떤 인터페이스나 클래스로 생성된 인스턴스가 아니기 때문에 연산을 실행해보면 항상 false이다.

# Assignment Operator

Assignment Operator(대입 연산자)는 다음과 같이 사용된다.

```[Type] [variable] = [value]``` 오른쪽 `value`를 `Type`에 맞게 `variable`(변수)에 대입(Assignment)한다.

# 화살표 연산자(Arrow Operator)

Java 8 부터 도입된 연산자이다. `Lambda Expression`이라고 하고, 익명클래스를 대신해서 사용할 수 있도록 만들었다.

함수형 프로그래밍언어의 장점을 착안한다. 람다 표현식을 사용하면 `일급 객체`를 만들 수 있다. 이에 대해서는 다음에 정리해보겠다.

# 3항 연산자

3항 연산자로 `if~else`구문을 축약해서 사용할 수 있다.

기존 `if~else`구문

```java
int max = 0;

if (a > b) {
    max = a;
} else {
	max = b;    
}
```

3항연산자

```java
int max = a > b ? a : b;
```

# 연산자 우선 순위

연산자 우선 순위(Operator Precedence)

| 연산자               | 순위                                    |
| -------------------- | --------------------------------------- |
| postfix              | expr++, expr--                          |
| unary                | ++expr, --expr, + expr, -expr, ~,!      |
| multiplicative       | *, /, %                                 |
| additive             | +, -                                    |
| shift                | <<, >>, >>>                             |
| relational           | <, <=, >, >=, instanceof                |
| equality             | ==, !=                                  |
| bitwise AND          | &                                       |
| bitwise exclusive OR | ^                                       |
| bitwise inclusive    | \|                                      |
| logical AND          | &&                                      |
| logical OR           | \|\|                                    |
| ternay               | ? :                                     |
| assignment           | =,+=,-=,*=,/=,%=,&=,^=,\|=,<<=,>>=,>>>= |

# java switch 구문

`switch` 구문은 다중if문과 같다. `if ~ else if ~ else`를 `switch`문으로 표기 가능하다.

요일을 숫자로 표시하는 구문을 작성해보자.

```java
public enum Day {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY;
}
```

```java
int numberLetters = 0;
Day day = Day.WEDNDESDAY;
switch (day) {
    case MONDAY:
    case FRIDAY:
    case SUNDAY:
        numberLetters = 6;
        break;
    case TUESDAY:
        numberLetters = 7:
        break;
    case THURSDAY:
    case SATURDAY:
        numberLetters = 8:
        break;
    case WEDNESDAY:
        numberLetters = 9:
        break;
    default:
        throw new IllegalStateException("Invalid day: " + day);
}

System.out.println(numberLetters);
```

java 12 버전부터는 화살표 연산자(`->`)를 사용하여 `break`문 없이 사용할 수 있다.

``` java
Day day = Day.WEDNESDAY;
int numberLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY                -> 7;
    case THURSDAY, SATURDAY     -> 8;
    case WEDNESDAY              -> 9;
    default -> throw new IllegalStateException("Invalid day: " + day);
}
```

위 처럼 `switch`구문(statement)를 `switch` 식(expression)으로 표기가 가능해졌다.

그리고 java13에서는 `yield`를 통해 switch식을 사용할 수 있다.

```java
Day day = Day.WEDNESDAY;
int numberLetters = switch (day) {
    case MONDAY:
    case FRIDAY:
    case SUNDAY:
        System.out.println(6);
        yield 6;
    case TUESDAY:
        System.out.println(7);
        yield 7;
    case THURSDAY:
    case SATURDAY:
        System.out.println(8);
        yield 8;
    case WEDNESDAY:
        System.out.println(9);
        yield 9;
    default:
        throw new IllegalStateException("Invalid day: " + day);
}
```

* `switch statement`는 `break`
* `switch expression`은 `yield`
* `break`, `yield`등은 `colon case`에서만 사용한다.

> 참고: 오라클 공식문서에서는 "arrow case" 사용을 추천하고 있다. "colon case"에서 break, yield구문을 빼먹기 쉽기 때문이다.

# 참고자료

* <https://docs.oracle.com/javase/tutorial/java/nutsandbolts/operators.html>


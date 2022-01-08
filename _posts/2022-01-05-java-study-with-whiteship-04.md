---
title: 자바스터디 4주차
excerpt: "4주차 과제: 자바 제어문"
author_profile: true
categories:
  - Java
tags:
  - Java
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2022-01-05T21:00-22:00
header:
  teaser: "/assets/images/java-teaser.jpg"
---

# 개요🙌

**목표:** 자바가 제공하는 제어문을 학습하자.

**학습할 것**

* 선택문
* 반복문

프로그램언어는 제어문을 통해 프로그램을 실행하는데, `Java`의 제어문은 **선택문**, **반복문**, **분기문**으로 나뉜다.

# 선택문

`Java`에서 제공하는 선택문은 다음과 같다.

* `if ([condition]) then`
* `if ([condition]) then else`
* `switch`

**if ([condition]) then**

```java
class IfThen {
    public static void main(String[] args) {
        int number = 10;
        
        if (number % 2 == 0) {
            System.out.println("짝수");
        }
    }
}
```

**if ([condition]) then else**

```java
class IfThenElse {
    public static void main(String[] args) {
        int testscore = 76;
        char grade;

        if (testscore >= 90) {
            grade = 'A';
        } else if (testscore >= 80) {
            grade = 'B';
        } else if (testscore >= 70) {
            grade = 'C';
        } else if (testscore >= 60) {
            grade = 'D';
        } else {
            grade = 'F';
        }
        System.out.println("Grade = " + grade);
    }
}
```

**switch**

`switch`문은 `if-then-else`를 열거한 형태로 `switch`문의 변수로는 `byte`,`short`,`char`,`int` 등 `Primitive`타입과 `enumerated`타입을 사용할 수 있다. `switch`문의 경우, 이전 [자바스터디 3주차]()에서 `enumerated`타입을 사용하는 예제를 보였다. 

```java
public class SwitchDemo {
    public static void main(String[] args) {

        int month = 1;
        String monthString;
        switch (month) {
            case 1:  monthString = "1월";
                     break;
            case 2:  monthString = "2월";
                     break;
            case 3:  monthString = "3월";
                     break;
            case 4:  monthString = "4월";
                     break;
            case 5:  monthString = "5월";
                     break;
            case 6:  monthString = "6월";
                     break;
            case 7:  monthString = "7월";
                     break;
            case 8:  monthString = "8월";
                     break;
            case 9:  monthString = "9월";
                     break;
            case 10: monthString = "10월";
                     break;
            case 11: monthString = "11월";
                     break;
            case 12: monthString = "12월";
                     break;
            default: monthString = "Invalid month";
                     break;
        }
        System.out.println(monthString);
    }
}
```

다음과 같이 `int`타입인 `month`변수가 `switch()`문 괄호 안의 변수로 들어가게 되고, `case`문의 조건에 맞는 값을 표현한다.

* **break문 생략 주의**

`switch`문을 작성하다 보면 `break`문을 빼먹게 되면 예상하는 결과와 다른 결과가 나타날 수 있다.

![without_break_in_switch](\assets\images\java-study\4\without_break_in_switch.png)

위 코드에서 break를 다 생략한 했을 때, `switch`문을 빠져나가지 못하고, `default` 경우까지 도달하게 되어 `monthString`에 "Invalid month"란 `String`값이 들어가게 되었다.

그래서, `Java`12버전부터 화살표 연산자(`->`)를 통해 `break`문의 따로 입력하지 않아도 된다.

# 반복문

반복문은 조건이 참(true)인 경우, Scope 내에 있는 명령어들을 반복적으로 실행한다.

`Java`에서는 대표적으로 `while`,`do-while`,`for` 세 가지 표준 순환 구조가 있다.

## while

```java
while (expression) {
    statement(s);
}
```

`while`문 괄호 안에 `expression`값은 `boolean`타입으로 값이 `true`인 경우, 블록({})안에 들어 있는 `statement`문을 실행한다.

```java
class WhileDemo {
    public static void main(String[] args) {
        int count = 1;
        while (count < 11) {
            System.out.println("Count is " + count);
            count++;
        }
    }
}
```

위 `WhileDemo`를 실행하면, count가 1씩 증가하면서 10까지 찍히는 것을 볼 수 있다.

`while`문 조건으로 `true`를 넣게 되면 무한루프가 되어 버린다. 따라서 특정 조건을 넣어서 `break`문을 통해 `while`문을 빠져 나올 수 있도록 해야 한다.

```java
int count = 1;

while (true) {
    System.out.println("Count is " + count);
    count++;
    if (count > 10) {
        break;
    }
}
```

## do-while

`do-while`문과  `while`문 차이는 `do-while`문은 무조건 한 번은 실행된다는 것이다.

`do-while`문의 문법은 다음과 같다.

```java
do {
    statement(s);
} while (expression);
```

`while`문은 블록 상위에 있지만, `do-while`문의 경우 블록 후에 나타나는 것을 볼 수 있다. 

## for

`for`문은 특정 범위 조건에 맞게 명령을 반복 실행한다.

`for`문 문법은 다음과 같다.

```java
for (initialization; termination; increment) {
    statement(s);
}
```

* initialization: 초기값을 지정한다.
* termination: `for`문이 반복 실행할 수 있도록 조건을 작성한다. 만약 `false`라면 `for`문을 종료한다.
* increment: `for`문의 블록 내 명령을 다 수행하고 실행되는 명령문으로, `increment`라고 표현했지만, `decrement` 감소 연산을 넣을 수 있다.

```java
class ForDemo {
    public static void main(String[] args) {
        for (int i = 0; i < 10; i++) {
            System.out.println("Count is " + i);
        }
        
        for (int i = 10; i >= 0; i--) {
            System.out.println("Count is " + i);
        }
    }
}
```

`for`문을 다음과 같이 작성하면 무한루프를 만들 수 있다.

`for (;;)`

## Enhanced For

향상된(?) `for`문은 Java 5.0부터 지원하는 `for`문으로 기존과 다른 문법을 가진다.

이 문법은 `Collections`, `Array` 객체들만 사용할 수 있다.

**기존 array for문 vs 향상된 for문**

```java
class NormalForDemo {
    public static void main(String[] args) {
        int[] numbers = {1,2,3,4,5,6,7,8,9,10};
        
        // 1부터 10까지 순회
        for (int i = 0; i < numbers.length; i++) {
            System.out.println("number is " + numbers[i]);
        }
        
        for (int number : numbers) {
            System.out.println("numbers is " + number);
        }
    }
}
```

**Collections객체사용예**

```java
class EnhancedForDemo {
    public static void main(String[] args) {
        List<Integer> numbers = new ArrayList<>();
        numbers.add(1);
        numbers.add(2);
        numbers.add(3);
        numbers.add(4);
        numbers.add(5);
        
        for (int number : numbers) {
            System.out.println(number);
        }
    }
}
```

`Collections`객체도 마찬가지로 다음처럼 향상된 `for`문을 사용할 수 있다.

`Java`에서 제공하는 `Collection`이란 인터페이스를 구현(implements)한 객체는 다음 사진과 같다.

![colls-coreInterfaces](\assets\images\java-study\4\colls-coreInterfaces.gif)

`Collection` 인터페이스를 확인해보면, `Iterable` 인터페이스를 상속(extends)받는 것을 볼 수 있다. `Iterable` 인터페이스를 상속받은 객체는 향상된 `for`문에서 사용 가능하다.

![collection_extends_Iterable](\assets\images\java-study\4\collection_extends_Iterable.png) 

**향상된 `for`문의 장점**

* 배열이나 `Collection`객체의 크기를 모르더라도 사용가능하다.
* 반복문 내에서 범위를 신경쓰지 않아도 되므로 반복문 구현에 집중할 수 있다.

**단점**

* 반복문의 값은 반복문 내에서만 사용가능하다.
* 객체의 실제 값을 변경할 수 없다.(Immutable)

# 정리

자바에서 제공하는 순환문, 반복문에 대해 공부하여 작성해 보았다.

해당 내용에 대해서 잘 알고 있다고 생각했는데, 막상 글로 풀어 쓸려고 하니까 되게 어렵게 느껴졌다. 이를 통해 책을 작성하시는 분들의 대단함을 느꼈다.

# 참고자료

* <https://docs.oracle.com/javase/tutorial/java/nutsandbolts/operators.html>


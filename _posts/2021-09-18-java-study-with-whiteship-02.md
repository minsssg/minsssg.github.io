---
title: 자바스터디 2주차
excerpt: "2주차 과제: 자바 데이터 타입, 변수 그리고 배열"
author_profile: true
categories:
  - Java
tags: [Java,Primitive,Literal,Promotion]
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2021-09-18T21:00-22:00
header:
  teaser: "/assets/images/java-teaser.jpg"
---

# 개요🙌

백기선님의 온라인 과제 2주차 주제는 자바의 프리미티브 타입, 변수 그리고 배열을 사용하는 법이다. 학습할 내용은 다음과 같다.

* 프리미티브 타입 종류와 값의 범위 그리고 기본 값
* 프리미티브 타입과 레퍼런스 타입
* 리터럴
* 변수 선언 및 초기화 하는 방법
* 변수의 스코프와 라이프 타임
* 타입 변환, 캐스팅 그리고 타입 프로모션
* 1차 및 2차 배열 선언하기
* 타입 추론, var

학습할 내용을 보면 아는 내용도 있고 생소한 단어도 있는 것 같아 차근차근 공부해보려 한다.

# 프리미티브 타입 종류와 값의 범위 그리고 기본 값

먼저, 프리미티브(Primitive)의 사전적 정의를 검색해보면, `원시 사회의, 초기의` 라는 의미로 `primitive type`이라고 하면 `원시적인 타입`? 이라고 해석이 되는데, 사람으로 치면 아담과 이브, 오스트랄로 피테스쿠스처럼 자바에서 제공하는 가장 기본적인 타입이란 것이다.

프리미티브 타입 종류는 다음과 같다. 프리미티브 타입 종류와 설명은 이전에 한 번 정리한 적이 있다. 하지만 복습 차원에서 다시 해보려 한다.

Primitive Type은 총 8개이다.

|  이름   |           크기           |                        값 범위                         |  기본값  |                             비고                             |
| :-----: | :----------------------: | :----------------------------------------------------: | :------: | :----------------------------------------------------------: |
|  byte   |       1byte(8bits)       |                -128~127(-2^3 ~ 2^3 -1)                 |    0     |                                                              |
|  short  |      2bytes(16bits)      |            -32,768~32,767(-2^15 ~ 2^15 -1)             |    0     |                                                              |
|   int   |     4bytes(32bitis)      |            -2,147,4783,648 ~ 2,147,483,647             |    0     |        unsigned int 는 Integer 클래스를 사용해야 함.         |
|  long   |      8bytes(64bits)      | -9,223,372,036,854,755,808 ~ 9,223,372,036,854,775,807 |    0L    |          unsigned long은 Long 클래스를 사용해야 함.          |
|  float  |      4bytes(32bits)      |                single-precision* 32-bit                |   0.0f   | 통화(currency)같은 정확한 값을 구해야 할 때, float 타입을 쓰지마라. |
| double  |      8bytes(64bits)      |                double-precision* 64-bit                |   0.0d   |                                                              |
| boolean |           1bit           |                       true/false                       |  false   |                                                              |
|  char   | 2bytes(8bits) - 유니코드 |            '\u0000' ~ '\uffff' (0 ~ 65,535)            | '\u0000' |                                                              |

* 위의 8개 primitive타입 말고 자바에서 `java.lang.String`이란 클래스를 지원한다.
  * 단순히 ""(double quote)를 사용하여 문자열을 정의할 수 있다.
  * `String`클래스는 기술적으로는 프리미티브 타입이 아니지만, 자바에선 프리미티브 타입으로 간주하고 있다.
  * `String`객체는 `불변성(immutable)`으로 한 번 만들어지면 값을 바꿀 수 없다. 이에 대한 내용은 추후에 정리하자. 
* Java 8버전에선 unsigned int, long을 지원한다. 하지만, `int, long` 타입에 직접적으로 `unsigned int, long`의 최댓값을 넣으면 컴파일 에러가 난다. 이 때, `Integer, Long`과 같은 `Wrapper Class`를 사용해야 한다.

![java8unsingedTest](/assets/images/java-study/2/java8unsingedTest.png)

![java8unsignedTest2](/assets/images/java-study/2/java8unsignedTest2.png)

* 통화(Currency)와 같은 타입에선 `float`타입을 사용하지 마라고 Oracle 공식 문서에 나와있다.
  * `java.math.BigDecimal` 클래스를 사용하라고 나와 있다.
  * `BigDecimal`을 사용해야 하는 이유는 추후 알아보자

# 프리미티브 타입과 레퍼런스 타입

프리미티브 타입과 레퍼런스 타입은 결국 변수가 어떤 타입인지를 정의하는데, 우선 변수는 값을 담는 메모리의 공간이다.

`Primitive type` vs `Reference type`의 차이점은 값을 저장하는지 주소를 저장하는지이다.

* Primitive type
  * 값 저장, `Object`가 아니다.
  * 변수에 지정한 값을 저장한다.
* Reference type
  * 참조 주소를 저장, `Object`이다.
  * 변수에 참조하는 객체의 메모리 주소값을 저장한다.

# 리터럴

> literal - 문자 그대로의

위 literal이란 단어의 뜻으로 "문자 그대로의"란 의미로  primitive에 들어가는 값 자체를 의미한다.

생각해보면 클래스 만들고 객체를 생성할 때, `new`라는 키워드를 사용한다. 하지만, 프리미티브 타입을 초기화(initialize)를 할 때, `new` 키워드를 사용하지 않는다.

```java
boolean result = true;
char character = 'A';
int luckyNumber = 7;
String hello = "Hello World";
```

위의 코드처럼 프리미티브 타입을 초기화할 때, 단순 값을 집어 넣는데, 이 값을 리터럴이라고 부른다.

# 변수 선언 및 초기화 하는 방법

프리미티브 타입 변수를 선언하는 방법은 `프리미티브 타입` + `변수 명`  으로 선언한다.

프리미티브 타입 변수를 초기화하는 방법은 프리미티브 타입마다 초기화할 때, 작성하는 리터럴 규칙이 있다.

## Integer Literals

```java
// 26 정수를 10진법, 16진법, 2진법으로 표현.
int decimalValue = 26;
int hexaValue = 0x1a;
int binaryValue = 0b11010;
```

`byte, short`은 `Integer Literals` 로 초기화 할 수 있다.

`long`타입도 `Integer Literals`로 초기화 할 수 있지만, `int`형의 최대값보다 큰 경우에는 숫자 뒤에 알파벳 `L`을 붙여주어야 한다.

```java
long longValue = 1L;
```

![long-value-without-L](/assets/images/java-study/2/long-value-without-L.png)

위 사진을 보면 숫자 `100억`은 `int`의 최대값인 `2^31-1` 값보다 크기 때문에 `Integer number too large`라는 것을 볼 수 있다.

![long-value-with-L](/assets/images/java-study/2/long-value-with-L.png)

다음과 같이 `100억`숫자 뒤에 알파벳`L`을 붙이니 초기화가 되었다.

> 참고: 자바 오라클에선 `long`타입 데이터를 선언할 때, 소문자 `l`을 사용해도 되지만, 문자 포튼에 따라서 `l`이 숫자 `1`과도 헷갈리 여지가 있기 때문에 대문자 `L` 사용을 추천하고 있다.

## Floating-Point Literals

Floating-Point 타입은 `float`, `double`  두 가지가 있는데, `float`은 `f` 또는 `F`를 숫자 끝에 필수로 작성해야 하고, `double`은 `d`또는 `D` 를 선택적으로 넣는다.

![float-value-unit-test](/assets/images/java-study/2/float-value-unit-test.png)

![float-value-unit-test-result](/assets/images/java-study/2/float-value-unit-test-result.png)

## Character와 String Literals

`char`와 `String` 타입은 유니코드(UTF-16)의 글자로 표현된다.

## Class Literals

자바에는 `Class Literals`이란 리터럴이 있는데, 리터럴 사용하는 방법은 `타입 명` + `.class`를 붙이면 된다.

```java
Class stringClass = String.class;
```

>  `Class`클래스에 대해서도 추후에 정리해보자

## 언더스코어(Underscore) 

언더스코어("_") 문법은 java 7버전부터 도입 되었다. 언더스코어는 숫자 리터럴에서 숫자 사이에 언더스코어를 삽입할 수 있는데, 큰 수를 표현하는데 가독성에 도움을 준다.

```java
int tenBillion = 1_000_000_000;
```

![java7-underscore](/assets/images/java-study/2/java7-underscore.png)

컴파일에러가 전혀 없다. 큰 숫자의 가독성을 높여줘서 정말 좋은 것 같다.

# 변수의 스코프와 라이프 타임

Scope뜻을 검색해보면 범위란 뜻으로 해당 변수를 사용할 수 있는 범위, 공간을 의미한다. 변수에 접근할 수 있는 범위를 말한다.

java에선 `{}`중괄호로 그 범위를 나누고 있다.

스코프의 종류는 다음과 같다.

## Class Scope

클래스 범위로 클래스 내에서 선언된 변수는 클래스 내 어디서든 접근할 수 있다.

```java
public class ClassScopeExample {
    private int amount = 0; // Member Variables
    
    public void incremential() {
        ammount++;
    }
    
    public void addNumber(int number) {
        amount = amount + number;
    }
}
```

## Method Scope

메서드 스코프는 매서드 내에 선언된 변수를 말한다. 해당 변수는 선언된 매서드 내에서만 접근이 가능하다.

```java
public class MethodScopeExample {
    public void methodA() {
        int intValue = 10;
    }
    
    public void methodB() {
        // compiler error, cannot resolve symbol
        System.out.println(intValue);
    }
}
```

## Loop Scope

`for`, `while`등 내에서 사용하는 범위를 말한다.

```java
public class LoopScopeExample {
    List<String> listOfNames = Arrays.asList("Joe", "Susan", "Pattrick");
    public void iterationOfNames() {
        String allNames = "";
        for (String name : listOfNames) {
            allNames = allNames + " " + name;
        }
        // compiler error
        String lastNameUsed = name;
    }
}
```

`name`이란 변수는 `for`문에서만 사용할 수 있다.

## Bracket Scope

Bracket이란 괄호란 의미를 갖는데, 즉 중괄호`{}`를 의미한다. 

단순히 중괄호 내에서 변수를 선언하면, 중괄호 내에서 밖에 사용하지 못한다.

## Scopes와 변수 Shadowing

만약 클래스의 맴버 변수와 같은 이름을 갖는 메서드 변수를 사용하게 되면 어떻게 될까?

![shadowing](/assets/images/java-study/2/shadowing.png)

![shadowing-result](/assets/images/java-study/2/shadowing-result.png)

결과를 확인해보면 처음 `title`값은 `Variables Shadowing`을 출력하고 두 번째 `title`은 `After Shadowing`이란 값으로 초기화된 `title`을 출력했다.

이렇게 같은 변수명을 사용하는 것을  `Variables Shadowing`이라고 한다. 하지만 `Variables Shadowing`은 혼란을 야기한다. 따라서 클래스 변수(맴버 변수) 앞에 `this`를 붙여서 **구분**을 하는 것 이 좋다.

# 타입 변환, 캐스팅 그리고 타입 프로모션

타입 변환(type conversion)은 말 그대로 타입을 변환하는 것이다. 타입 변환은 데이터 타입이 컴파일러에 의해 컴파일 시간에 다른 데이터 타입으로 자동 변환된다. 밑의 코드처럼 말이다.

```java
int myInt  = 127;
long myLong = myInt;

float myFloat = myLong;
double myDouble = myLong;
```

![float-type-casting](/assets/images/java-study/2/float-type-casting.png)

아래 테스트 코드를 진행하면 SUCCESS 결과를 보여주는데, `float`타입의 경우, 4바이트이므로 `long`타입으로 값을 초기화할 경우, 값이 변질 되는 것을 볼 수 있다. 

타입 캐스팅은 타입을 선정하는 것(Casting)으로 값 앞에 `()`안에 캐스팅할 타입을 넣어주는 것이다.

```java
float x = 3.141592f;
byte y;
y = (byte) x;
```

타입변환과 캐스팅의 큰 차이는 개발자가 `casting operator`를 사용 여부이다.

그러면 타입 프로모션은 무엇일까?

타입 프로모션은 자동으로 타입 변하는 것을 말하고, 묵시적 형변환을 말한다. 작은 타입이 큰 타입으로 변환할 때, 데이터 앞에 따로 캐스팅 연산자를 넣을 필요가 없는데, 이를 `Promotion`이라고 한다.

> 자바 형 변환에 대해서 좋은 글이 있는데, 시간이 되면 일어보자. https://webdevtechblog.com/%EC%9E%90%EB%B0%94-%ED%98%95%EB%B3%80%ED%99%98-casting-promotion-%EA%B3%BC-%EB%B0%94%EC%9D%B8%EB%94%A9-binding-ef3e453eb8a6

# 1차 및 2차 배열 선언하기

배열(Array)은 하나의 타입을 고정된 수의 값을 보유하는 객체이다. 배열을 길이를 갖는데, 배열이 만들어 질 때, 그 길이가 정해지고 한 번 정해진 길이는 고정이다.

![objects-tenElementArray](/assets/images/java-study/2/objects-tenElementArray.gif)

**출처: [docs.oracle.com](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/arrays.html)**

1,2차원 배열은 다음과 같이 선언할 수 있다.

```java
int[] intArray = new int[10]; // 10개의 int를 갖는 배열 생성
int[][] intDoubleArray = new int[10][10]; 
```

# 타입 추론, var

타입 추론(Type Inference)란, 컴파일 시점에서 변수의 타입을 유추하는 것을 말하는데, java5버전의 Generic에서 타입 추론을 하고, java8에서 Lambda식을 작성할 때 타입 추론을 사용한다.

그리고 java 10버전부터 추가된 `var`이란게 있는데, 이것은 로컬 변수를 선언할 때 사용할 수 있다.

`{}` 괄호안에서 사용이 가능하다.



```java
//java 10 이전
String message = "Java 10 Version 이전";
Map<Integer, String> map = new HashMap<>();
//java 10 이후
var message  = "Java 10 Version부터";
var idToNameMap = new HashMap<Integer, String>();
```

## var 사용 시 주의점

* 초기값 없으면 컴파일 에러 발생
* null로 초기화 할 수 없다.
* 람다 표현식에 사용할 수 없다.
* 타입없는 배열을 담을 수 없다.

# 참고자료

* [https://www.programcreek.com/2013/03/arraylist-vs-linkedlist-vs-vector/](https://www.programcreek.com/2013/03/arraylist-vs-linkedlist-vs-vector/)
* [https://www.baeldung.com/java-10-local-variable-type-inference](https://www.baeldung.com/java-10-local-variable-type-inference)


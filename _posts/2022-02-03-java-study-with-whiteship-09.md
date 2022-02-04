---
title: 자바스터디 9주차
excerpt: "9주차 과제: 예외 처리"
author_profile: true
categories:
  - Java
tags: [Java, Exception]
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2022-02-03T21:00-22:00
header:
  teaser: "/assets/images/java-teaser.jpg"
---

# 개요🙌

**목표:** 자바의 예외 처리에 대해 학습하자.

**학습할 것**

* 자바에서 예외 처리 방법(try, catch, throw, throws, finally)
* 자바가 제공하는 예외 계층 구조
* Exception과 Error의 차이
* RuntimeException과 RuntimeException이 아닌 것의 차이는?
* 커스텀한 예외 만드는 방법

자바에서 예외(Exception)는 **프로그램 오류(에러)**를 말한다. 프로그램 오류는 프로그램이 실행 했을 때, 어떠한 원인에 의해 프로그램이 작동하지 않거나, 비정상적으로 종료되는 것을 말한다.

프로그램 에러가 발생하는 시점으로 오류를 다음과 같이 나눌 수 있다.

* 컴파일 에러 - 컴파일 시 발생하는 에러
* 런타임 에러 - 프로그램 실행 시 발생하는 에러
* 논리적 에러 - 실행은 되지만, 의도와는 다르게 동작하는 것

![program-error](\assets\images\java-study\9\program-error.png)

컴파일 에러(Compile Error)는 소스코드를 컴파일(javac) 할 때, 문법(Syntax)를 검사해서 에러가 있는지 확인한다.(javac로 따로 컴파일하지 않아도 IDE에서 컴파일 에러를 확인해줌.) 타입이나 변수 선언, 접근제어자 위치가 잘못된 곳에 작성하면 컴파일 에러가 발생한다.

컴파일을 성공적으로 마치고 나면, `.class`파일이 만들어지고, `JVM`에서 해당 클래스 파일을 실행한다. 프로그램을 실행했을 떄, 프로그램 오류가 발생할 수 있는데, 이를 런타임 에러(Runtime Error)라고 한다.

자바에서 런타임 에러는 두 가지 분류로 나뉜다. `에러(Error)`와 `예외(Exception)` 두 가지로 구분한다.

`에러`는 메모리 부족(OutOfMemoryError)나 스택 오버플로우(StackOverflowError)와 같이 발생하면 프로그램을 복구할 수 없는 **심각한** 에러이다. 반면에 `예외`는 **예외 처리**를 통해 수습할 수 있는 에러를 말한다.

* 에러(Error) : 프로그램 코드에 의해서 수습될 수 없는 심각한 오류
* 예외(Exception) : 프로그램 코드에 의해서 수습될 수 있는 오류

자바에서는  예외를 처리할 수 있도록 미리 정의해둔 **예외 클래스**들이 있다. 이에 대해 알아보자.

> 논리적 에러: 컴파일 에러도 아니고 런타임 에러도 아니다. 단순히 우리가 논리적 연산을 잘못해서 원치 않은 결과를 출력하는 것을 논리적 오류라고 한다.

# 자바가 제공하는 예외 계층 구조

자바에서 예외 처리할 수 있도록 예외 클래스들을 정의해두었다.

![exception-handling](\assets\images\java-study\9\exception-handling.png)

위 그림을 보면, `Error`와 `Exception` 클래스는 `Throwable`이란 객체를 상속받고 있다.(`Throwable`이라 길래 당연히 인터페이스인줄 알았다...) 

에러 클래스에 대해서는 넘어가고, 예외 클래스를 보면 `Exception`클래스가 최상위 클래스인 것을 볼 수 있다. `Exception`은 또한, `RuntimeException`클래스와 그 이외의 `Exception`클래스로 분류할 수 있다.

`RuntimeException`클래스는 프로그래머가 코드 작성 시 실수를 통해 발생될 수 있는 예외를 말한다. 예를 들어, 배열의 인덱스 범위를 초과(IndexOutofBoundsException)했거나, 변수가 `null`을 참조하는 맴버변수나 메서드를 호출할 때(NullPointerException), 클래스 간에 형변환을 잘못했을 때(ClassCastException), 정수를 0으로 나눌려고 할 때(ArithmeticException), 발생한다.

그 이외의 `Exception`클래스는 프로그램 사용자에 의해 발생하는 경우가 많다. 예를 들어, 없는 파일을 사용하려하면, (FileNotFoundException), 클래스의 이름을 잘못 적은 경우(ClassNotFoundException), 입력한 데이터 형식이 이상한 경우(DataFormatException) 발생한다.

* RuntimeException클래스들 - 프로그래머의 실수로 발생하는 예외
* Exception 클래스들 - 사용자의 실수와 같은 외적인 요인에 의해 발생하는 에외

# Exception과 Error의 차이

위에서 이야기한 것처럼 `Throwable` 클래스를 상속받은 `Exception`과 `Error` 클래스가 있다. 하지만, `Error`클래스가 발생하면 해당 클래스는 예외처리를 할 수 가 없다. 그리고 `Error`클래스를 상속받아서 사용하지 말라고 권장한다.(이펙티브 자바에서)

`Exception`클래스는 예외 처리가 가능한 클래스들로 이를 다음과 같이 구분할 수 있다.

* `Checked Exception or Compile Time Exception`
* `Un-Checked Exception or Runtime Exception`

**RuntimeException은 Checked Exception과 Un-CheckedException을 구분하는 기준이다**

이 두 `Exception`의 가장 큰 차이는 **반드시** 예외 처리를 해야 되는 가 처리하지 않아도 되는가 이다.

## Checked Exception

 `Checked Exception`은 `Exception`클래스들 중 컴파일 시간에 체크해야 하는 `Exception`클래스이다. 여기서 체크한다는 말은 `try catch`구문으로 예외 처리에 대한 로직을 작성해주어야 한다는 말이다.

**FileReader 데모**

```java
public class Main {
    public static void main(String[] args) {
        
        try {
            FileReader fileReader = new FileReader("./test.txt");
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

파일을 읽을 때, 파일이 없을 수 있기 때문에 항상 `try - catch`구문으로 `FileNotFoundException`클래스로 예외 처리 로직을작성해야 한다.

그렇지 않으면, 컴파일 타임에서 에러를 출력하게 된다. 그래서 `Checked Exception`클래스를 `Compile Time Exception`이라고도 한다.

## Un-Checked Exception

반대로 모든 `RuntimeException`클래스들은 `Un-Checked Exception`으로 컴파일 타임에서 오류로 인식하지 않는다.

```java
public class Main {
    public static void main(String[] args) {
        int result = 30 / 0; // by Zero
    }
}
```

위 `main`매서드를 실행하면 숫자 30을 0으로 나눌려고 하기 때문에 `ArithmeticException` 예외가 발생한다. 즉, 컴파일 에러가 아닌 런타임 에러가 발생하는 것이다.

```java


public class CocaColaVendingMachine implements VendingMachine {

    @Override
    public Cola getCola(int price) {

        if (price < Cola.price) {
            throw new IllegalStateException("코카 콜라 금액보다 작습니다.");
        }

        return new Cola();
    }

    @Override
    public void payByCard() {
        System.out.println("카드로 결제");
    }

    @Override
    public void payByCash() {
        System.out.println("현금으로 결제");
    }
}
```

# 자바 예외처리 예외 처리 방법

예외처리(Exception Handling)란 프로그램 실행 시 발생할 수 있는 예외에 대비한 코드를 작성하는 것이다. 예외처리의 목적은 프로그램의 **비정상적인 종료**를 막고, 프로그램이 정상적으로 실행할 수 있도록 유지하는 것이다.

자바에서는 `try-catch`구문을 통해서 예외처리를 한다.

```java
try {
    // 예외가 발생할 수 있는 코드
} catch (Exception1 e1) {
    // Exception1이 발생했을 경우, 이를 처리하기 위한 코드
} catch (Exception2 e2) {
    // Exception2이 발생했을 경우, 이를 처리하기 위한 코드
} finally {
    // 마지막에 무조건 실행되는 코드
}
```

`finally` 블럭은 작성해도 되고 작성하지 않아도 된다. `finally`는 `try`블럭에서 예외가 발생하든 발생하지 않고 정상적으로 처리가 되고 무조건 실행되는 블럭이다. 

`catch`블럭은 `try`블럭 내에 여러 `Exception`들이 발생할 수 있기 때문에, 여러 `catch`블럭을 작성할 수 있다.

하지만 주의 해야할 점은 **부모 Exception클래스가 자식 Exception 클래스보다 먼저 오면 안된다.**

![exception-hierachy](\assets\images\java-study\9\exception-hierachy.png)

왜냐하면, `try` 블럭에서 `NullPointerException` 혹은 `ArithmeticException` 등 여러 예외가 발생할 수 있기 때문에 거기에 맞는 예외 처리를 해주어야 한다. 하지만, `Exception` 등의 부모 클래스를 사용하게 되면, 당연히 부모 예외 처리 블럭으로 처리가 되기 때문에 뒤에 있는 `catch`블럭이 의미가 없어지기 때문이다. 

> 만약, 발생한 예외를 처리하지 못하면 어떻게 될까? => 프로그램은 비정상적으로 종료되며, 처리되지 못한 예외(uncaught exception)는 JVM의 "예외처리기(UncaughtExceptionHandler)"가 받아서 원인을 화면에 뿌려준다.

## 예외 처리 흐름

```java
class ExceptionExample {
    String str = null;
    public static void main(String[] args) {
        try {
            System.out.println("1");
            System.out.println("2");
            System.out.println(str.charAt(0));
            System.out.println("3");
            System.out.println("4");
        } catch (NullPointerException e) {
            System.out.println("NullPointerException 발생");
        }
    }
}

출력
1
2
NullPointerException 발생
```

다음과 같이 `str`변수에 `null`값을 넣었을 때, `str.charAt()`매서드를 사용하는 것을 볼 수 있다. 여기서 `str`변수는 `null`을 참조하고 있기 때문에, `charAt()`매서드 사용시 `NullPointerException`이 발생하게 된다.

이 때, 출력 결과를 보면, `str.charAt()` 이후의 명령들은 실행되지 않는다.

## 멀티 블럭

자바 7부터 도입된 문법으로 `catch`블럭을 `|` 기호를 통해 하나의 `catch`블럭으로 합칠 수 있다.

```java
class ExceptionExample {
    String str = null;
    public static void main(String[] args) {
        try {
            System.out.println("1");
            System.out.println("2");
            System.out.println(str.charAt(0));
            System.out.println("3");
            System.out.println(1 / 0); // ArithmeticException
            System.out.println("4");
        } catch (ArithmeticException | NullPointerException e) {
            System.out.println("NullPointerException 발생");
        }
    }
}
```

> 멀티 catch 블럭을 사용할 때, 예외 클래스가 조상과 자손의 관계에 있다면 컴파일 에러가 발생한다. 왜냐하면 조상과 자손이 같이 있을 때, 조상 예외 클래스만 써주면 되기 때문이다.

## throw

`throw`키워드는 프로그래머가 고의로 예외를 발생시킬 수 있는 키워드이다.

```java
class ThrowExample {
    public static void main(String[] args) {
        try {
        	throw new Exception("고의로 Exception발생");    
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

`throw new` 키워드를 통해 `Exception`을 생성할 수 있다. `Exception`생성 시 생성자 인자로 `String`을 넣을 수 있는데, `catch`블럭에서 변수 `e`의 `getMessage()`로 내용을 확인할 수 있다.

## throws

`throws` 키워드는 `throw`에 `s`가 붙은 것이지만 의미는 다르다.

`throw`는 고의로 예외를 발생시키는 것이고, `throws`는 **예외 회피**에 해당한다.

**예외 회피**란 `try-catch`로 예외 처리를 하지 않고, 해당 메서드를 호출한 곳으로 예외를 넘기는 것이다.

**사용법**

```java
public class ThrowsException {

    public static void main(String[] args) {
        try {
            Calculation.divide(10, 0);
        } catch (ArithmeticException e) {
            System.out.println(e.getMessage());
        }
    }
}

class Calculation {
    public static int divide(int a, int b) throws ArithmeticException {
        return a / b;
    }
}
```

`Calcuation.divide` 매서드 정의 옆에 `throws ArithmeticException`을 볼 수 있다. `ThrowsExample.main` 에서 `Calculation.divide(10,0)`을 호출했고, `divide`매서드 내에서 `ArithmeticException`이 발생했지만, `throws` 키워드를 통해 `divide`매서드를 호출한 `main`에서 `try-catch`로 예외 처리하는 것을 볼 수 있다.

## try-with-resources

자바 7 부터 `try-with-resources`문이 추가 되었다.

**MyResource**

```java
public class MyResource implements AutoCloseable {
    
    public void doSomething() {
        System.out.println("do something");
        throw new FirstError();
    }

    @Override
    public void close() {
        System.out.println("자원 반납");
        throw new SecondError();
    }
}
```

`try-with-resources`에 사용하려는 리소스 클래스들은 `AutoCloseable` 인터페이스를 구현해야 한다.

**FirstError**

```java
public class FirstError extends RuntimeException {}
```

**SecondError**

```java
public class SecondError extends RuntimeException {}
```

**Main**

```java
public class Main {
    
    public static void main(String[] args) {
        MyResource myResource = new MyResource();
        try {
            myResource.doSomething();
        } finally {
            myResource.close();
        }
    }
}
```

![skip-first-error](\assets\images\java-study\9\skip-first-error.png)

1. `myResource.doSomething()` 메서드에서 `FirstError`예외가 발생한다.
2. `finally`블럭의 `myResource.close()`매서드를 실행하면서 `SecondError` 예외가 발생한다.
3. 이 때, `FirstError` 예외에 대한 내용이 덮어진 것을 볼 수 있다. 이렇게 되면, 실제로는 예외가 두 군 대에서 발생했지만, 한 곳만 예외가 난 것처럼 보인다.

그렇다면 `try-with-resources`를 쓸 때는 어떨까?

```java
public class Main {

    public static void main(String[] args) {

        try (MyResource myResource = new MyResource();) {
            myResource.doSomething();
        }
    }
}
```

![try-with-resources-결과](\assets\images\java-study\9\try-with-resources-결과.png)

이전의 결과와 달리, `doSomething`매서드에서 `FirstError`가 난 이후, `close()`매서드가 실행된 것을 볼 수 있다. 이 때, `Suppressed`란 머리말과 함께 에러 내용이 출력되는 것을 볼 수 있는데, 이는 `try-with-resources`구문에서 `Throwable`배열로 해당 내용을 저장하기 때문에 예외가 덮어쓰워지지 않고 출력되는 것을 볼 수 있다.

**백기선님의 이펙티브 자바 참고 영상**

{% include components/youtubePlayer.html id="zqjZBSqHs0s" %}

# 커스텀 예외 만들기

사용자가 정의하는 예외를 만들기 위해서는 예외 클래스를 상속 받아야 한다.(extends)

* `Checked Exception`을 구현할 때, `Exception` 상속
* `Un-Checked Exception`을 구현할 때, `RuntimeException` 상속

```java
class MyException extends Exception {
    MyException(String message) {
        super(message);
    }
}
```

> 참고: 가능하면 새로운 예외 클래스를 만들기보다 기존의 예외 클래스를 활용하자.

# 정리

이번 9주차에서는 자바  예외에 대해서 다루었다.

* 프로그래밍 오류란 무엇인지
* 런타임 오류에 예외와 에러의 차이(예외는 복구 가능하지만, 에러는 복구 되지 않음)
* 예외를 처리하는 방법(try-catch, throws 예외 회피)
* 자바 7 이상이라면 `try-with-resources`를 사용하자
* 커스텀 예외 만드는 방법

# 참고자료

* 자바의 정석 3판
* <https://www.notion.so/3565a9689f714638af34125cbb8abbe8>
* <https://www.javaguides.net/2018/08/three-types-of-exceptions-in-java.html>


---
title: Java 제어자(Modifier)
excerpt: "Java 제어자(Modifier) 정리"
author_profile: true
categories:
  - Java
tags: [Java, static, public, private, final]
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2022-01-16T21:00-22:00
header:
  teaser: "/assets/images/java-teaser.jpg"
---

# 개요🙌

**목표:** [자바 스터디 7주차](https://minsssg.github.io/java/java-study-with-whiteship-07/)에서 접근 제어자(Access Modifier)에 대해 공부했다. 하지만, 제어자에 대한 정의와 제어자에는 접근 제어자 말고 다른 제어자에 대한 내용을 넣지 않았다.

* 접근 제어자: public, protected, default, private
* 그 외 제어자: static, final, abstract, native, transient, synchronized, volatile, strictfp

그래서, 오늘 제어자가 무엇인지와  접근 제어자 이 외의 제어자 중에서 `static`, `final`, `abstract` 제어자에 대해 알아보자.

# 제어자(Modifier)

제어자(Modifier)는 클래스, 변수 또는 메서드의 선언부에 사용되어 부가적인 의미를 더한다.

# static 

`static`키워드는 "클래스의" 또는 "공통적인"의 의미를 갖는다. `static`제어자 사용할 수 있는 곳은 다음과 같다.

* static 제어자 사용: 맴버변수, 메서드, 초기화 블럭

`static `제어자로 작정된 맴버변수를 "클래스변수"라고 하며, 일반 맴버변수는 "인스턴스변수"라고 한다. "인스턴스 변수"는 일반적으로 서로 다른 값을 가지지만, "클래스 변수"는 모든 인스턴스(객체)가 공유한다.

일반적으로 인스턴스 변수는 클래스를 `new`키워드로 객체를 만들 때, 메모리에 로드되지만, 클래스 변수는 클래스가 메모리에 로드 될 때, 생성된다.

따라서, "클래스 변수", "클래스 메서드"는 따로 객체는 생성(new)하지 않아도 사용할 수 있다는 것이다.

**StaticClass**

```java
public class StaticClass {

    static {
        price = 10000;
    }

    static int price;

    static int getDiscountPrice() {
        return (int) (price * 0.9);
    }
}
```

**ModifierMain**

```java
public class ModifierMain {

    public static void main() {

        System.out.println("원가: " + StaticClass.price);
        System.out.println("할인 가격: "  + StaticClass.getDiscountPrice());
    }
}

```

위 예시와 같이 `StaticClass`를 객체로 생성하지 않고 `price`와 `getDiscountPrice()`매서드를 참조해서 사용했다.

> 참고: 매서드 내에 인스턴스 변수를 사용하지 않는다면, `static `매서드로 사용하자.
>
> `static` 초기화 블럭은 클래스가 메모리에 로드될 때, 단 한번만 수행되고 주로 클래스 변수를 초기화할 때 사용된다.

# final

`final`에 대해서는 [자바 스터디 6주차](https://minsssg.github.io/java/java-study-with-whiteship-06/#final)에서 한 번 다뤘지만, 다시 정리했다.

`final` 제어자는 "마지막의", "변경할 수 없는" 의미를 가지는 제어자로 사용할 수 있는 곳은 다음과 같다.

* final 제어자 사용: 클래스, 메서드, 맴버변수, 지역변수

`final`제어자가 붙은 대상마다 기능이 다르다.

## final class

클래스에 `final`제어자가 붙으면, "변경될 수 없는 클래스"란 의미를 갖게 되며, 이는 상속(inheritanence)할 수 없다는 것이다.

**FinalClass**

```java
public final class FinalClass {

    final String className = "FinalClass";

    public void printMessage(String message) {

        System.out.println(className + " : " + message);
    }
}
```

**FinalSubClass**

```java
public class FinalSubClass extends FinalClass {
    // compile error
}
```

![final-class-not-inheritance](\assets\images\java-study\modifier\final-class-not-inheritance.png)

`FinalClass`는 `class` 앞에 `final `키워드가 붙으므로써, `final class`가 되었다. `FinalSubClass`에 `extends`를 통해 `FinalClass`를 상속받으려고 시도하면, IDE에서 컴파일에러를 보여준다.

## final 메서드

`final`메서드는 변경될 수 없는 메서드로, `final` 제어자가 붙은 메서드들은 `Override`재정의 할 수 없다.

**FinalMethodClass**

```java
public class FinalMethodClass {

    public void canOverrideMethod() {
        System.out.println("This is can Override Method");
    }

    public final void onlyUseFinalClass() {
        System.out.println("only use final class method");
    }
}
```

여기서, `canOverrideMethod()`메서드는 재정의할 수 있지만, `onlyUseFinalClass()`는 재정의할 수 없다.

![final-method-override](\assets\images\java-study\modifier\final-method-override.png)

## final 변수

`final` 변수는 변수 선언과 동시에 초기화를 해야하고, 초기화가 되면 값을 변경할 수 없다.

![final-member](\assets\images\java-study\modifier\final-member.png)

`NUMBER`, `SHAPE`는 `final`변수로 초기화만 할 시, 컴파일 에러가 난 것을 볼 수 있다.

**생성자를 이용한 final 맴버변수 초기화**

`final`변수는 선언과 동시에 초기화를 해야 하지만, 맴버변수는 생성자에서 초기화할 수 있다.

![card](\assets\images\java-study\modifier\card.png)

이런 식으로 생성자를 이용한 초기화를 사용하면, 객체 생성 시 필요한 값을 생성자 매개변수로 전달받을 수 있다. 생성자 매개변수로 전달 받지 않는다면, 모든 객체의 `NUMBER`, `SHAPE`은 같은 값을 사용해야 한다.

# abstract

`abstract`는 "추상의"라는 의미를 가지며, "미완성의"란 의미도 가지고 있다. `abstract`제어자는 "클래스"는 말그대로 "미완성된 클래스"란 뜻이다. 추상 클래스 역시 [자바스터디 6주차](https://minsssg.github.io/java/java-study-with-whiteship-06/#%EC%B6%94%EC%83%81-%ED%81%B4%EB%9E%98%EC%8A%A4abstract-class)에서 한 번 다루었다.

# 제어자의 조합

접근 제어자와 그 외 제어자는 서로 조합이 가능하다. 그리고 접근 제어자가 보통 먼저 온다.

## 제어자 조합 시 주의사항

1. 메서드에 `static`과 `abstract`를 함께 사용할 수 없다.
   * `static` 제어자는 구현된 메서드에서만 사용가능하다.
2. 클래스에 `abstract`와 `final`을 같이 쓸 수 없다.
   * `abstract`는 구현되지 않아 상속을 통해서 구현해야 되는 클래스이고, `final`은 더 이상 상속할 수 없다는 의미이기 때문에 서로 상충된다.
3. `abstract`메서드의 접근 제어자가 `private`이 될 수 없다.
   * `abstract` 메서드는 자식 클래스에서 구현해주어야 하는데, 접근 제어자가 `private`이면 자식 클래스에서 접근할 수 없기 때문이다.
4. 메서드가 `private`과 `final`을 같이 사용할 필요는 없다.
   * 접근 제어자가 `private`인 메서드는 `Override`될 수 없기 때문에 `final`과 `private`중 하나만 사용한다.

# 정리

오늘 배운 내용을 정리해보자.

* `static` 제어자는 클래스가 로드 할 때, 같이 메모리에 적재되어 객체를 생성하지 않아도 `static` 변수, 메서드를 사용할 수 있다.
* `final` 제어자는 각 대상마다 제한하는 대상이 다름.
* `abstract` 메서드가 있으면, 그 클래스는 무조건 `abstract`클래스
* 제어자 조합 주의사항!

# 참고자료

* 자바의 정석 3판


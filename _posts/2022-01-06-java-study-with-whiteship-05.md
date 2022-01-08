---
title: 자바스터디 5주차
excerpt: "5주차 과제: 클래스"
author_profile: true
categories:
  - Java
tags:[Java, Class, this]
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2022-01-06T21:00-22:00
header:
  teaser: "/assets/images/java-teaser.jpg"
---

# 개요🙌

**목표:** 자바의 Class에 대해 학습하자.

**학습할 것**

* 클래스 정의하는 방법
* 객체 만드는 방법 (new 키워드 이해하기)
* 메소드 정의하는 방법
* 생성자 정의하는 방법
* this 키워드 이해하기

객체지향 프로그래밍에서는 클래스(Class)는 객체의 청사진, 템플릿이라 하고, 쉽게 생각하면 붕어빵 틀이다. 클래스는 특정 상태(state)를 담고 있도록 하고, 특정 행위(behavior)를 할 수 있도록 정의한다.

# 클래스 정의하는 방법

자바에서 클래스는 객체의 정의, 유형(Type)을 나타낸다. 클래스는 `fields`, `constructors`, `methods`로 정의한다. `fields`로 특정 상태(state)를 담을 수 있고, `methods`를 통해 특정 행위(behavior)를 할 수 있도록 한다.

**클래스 선언**

```java
<access modifier> class ClassName {
    // fields, constructors
    // methods
}
```

클래스 선언은 `class`라는 키워드를 사용하고 뒤에 클래스명을 입력하고 블록({}) 내에 `fields`, `constructor`, `methods`를 입력한다.

**붕어빵 클래스**

```java
// class declaration
public class FishBread {

    // fields
    private String flavor;
    private String dough;
    private String topping;
    private int price;

    // constructor
    public FishBread(String flavor, String dough, int price) {
        this.flavor = flavor;
        this.dough = dough;
        this.price = price;
    }

    // method
    public void choiceTopping(String topping) {
        this.topping = topping;
    }

    public int getPrice() {
        return price;
    }

    @Override
    public String toString() {
        return "주문하신 붕어빵: [" + flavor + ", " + dough +"] 붕어빵은 가격 " + price + " 원 입니다.";
    }
}
```

클래스 명을 한글로도 쓸 수 있다. 하지만 한글로 사용하지 않는다.

**Naming Convention**

`class`를 선언할 때, 클래스명은 다음과 같은 Naming Convention을 지켜야 한다.

* 클래스 이름은 명사
* 첫 글자는 무조건 대문자(Captial Letter)
* 카멜 케이스([낙타 대문자](https://ko.wikipedia.org/wiki/%EB%82%99%ED%83%80_%EB%8C%80%EB%AC%B8%EC%9E%90))
* 축약어보단 명사 전체 단어를 다 작성해야 함.

# 객체 만드는 방법

클래스로부터 객체를 만들 때, `new`키워드로 객체를 생성한다.

```java
FishBread fishBread = new FishBread("슈크림", "기본반죽", 500);
```

`new`키워드로 객체를 생성할 때, 생성자(constructor) 형태에 맞게 생성할 수 있다.

**new 객체 생성 시 기억할 점**

1. `new`키워드로 객체 생성 시, runtime에 메모리에 할당된다.
2. 객체는 메모리 중 heap영역을 차지한다.
3. 생성자와 형태를 맞춰야 한다.

보통 객체를 생성할 때, `new`키워드로 생성하지만, 다른 방법도 있다.

## new 키워드 말고 다른 객체 생성 방법

* **Class.forName() 메서드**

자바에는 `java.lang` 패키지에 `Class`라는 미리 정의된 클래스가 있다. `Class`클래스를 이용하여 객체를 생성할 수 있다.

`Class.forName()`의 메서드 인자로 클래스패스를 넣으면 클래스 객체를 반환하는데, 클래스 객체 내에는 인자로 넘긴 클래스의 메타 정보(클래스이름, 클래스 생성자, 필드명, 메소드 명 등등)를 확인할 수 있다.

`Class`객체의 `newInstance`메서드를 통해서 객체를 생성하는 방법도 있지만, `deprecated`이다. 사용을 권장하지 않고 있다. 대신 생성자 클래스 `Constructor`클래스로 `newInstance`매서드를 통해 객체를 생성할 수 있다.

![class_constructor_newinstance](\assets\images\java-study\5\class_constructor_newinstance.png)

* **clone() 매서드**

`clone()`매서드를 사용하기 위해서는 해당 클래스가 `Cloneable` 인터페이스를 `@Override`해야 한다. 

![fish_bread_class_cloneable](\assets\images\java-study\5\fish_bread_class_cloneable.png)

이제 `clone`매서드를 사용해서 객체를 복사할 수 있다.

![create_object_clone](\assets\images\java-study\5\create_object_clone.png)

* **역직렬화(Deserialization)**

역직렬화를 통해서도 객체를 만들 수 있는데, 다음에 정리하도록 하겠다.

# 메소드를 정의하는 방법

메소드를 정의하는 방법은 다음과 같다.

```java
<access modifier> <return type> methodName(<parameter) {
    // @TODO
}
```

**Naming Convention**

1. 메소드는 행위를 나타내기 때문에 메서드 명에 동사가 포함되도록 작성한다.
2. 클래스와 다르게 첫 글자는 소문자로 작성하고 카멜 케이스를 따른다. 

# 생성자를 정의하는 방법

생성자는 객체를 생성할 때 호출되는데, 객체 생성의 초기화를 담당한다.

클래스를 정의할 때, 생성자를 필수로 들어가야 되며, 생성자를 따로 작성하지 않으면 default 생성자가 컴파일 타임에 작성된다.

```java
<access modifier> ClassName (<parameters>) {
    //initialization
}
```

# this

`this`는 자신 객체의 참조 변수이다. 그래서 생성자 또는 매서드 내에서 이름이 같은 변수지만, 앞에 `this`를 붙이면서, 구분할 수 가 있다.

또한 `this()`라는 코드도 볼 수 있는데, 이것은 자기 자신의 생성자를 의미한다. 또한, `super`도 있는데, 이것은 부모를 참조하는 변수로 상속 개념을 공부하고 정리하도록 하자.

# 참고자료

* <https://www.geeksforgeeks.org/classes-objects-java/>
* <https://docs.oracle.com/javase/tutorial/java/javaOO/classes.html>


---
title: 자바스터디 6주차
excerpt: "6주차 과제: 상속"
author_profile: true
categories:
  - Java
tags: [Java, 상속, super, Overriding, Dynamic Method Dispatch]
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2022-01-08T05:00-10:00
header:
  teaser: "/assets/images/java-teaser.jpg"
---

# 개요🙌

**목표:** 자바의 "상속"에 대해 학습하자.

**학습할 것**

* 자바 상속의 특징
* super  키워드
* 메소드 오버라이딩
* 다이나믹 메소드 디스패치(Dynamic Method Dispatch)
* 추상 클래스
* final 키워드
* Object 클래스

상속(inheritance)는 객체지향프로그래밍에서 사용되는 개념으로 자식 클래스가 부모 클래스의 기능을 받아쓰는 것을 말한다. 다른 표현으로 계승, 확장이라는 단어로도 사용된다. 

만화 "드래곤볼"을 보면 "초사이언"은 오직 "사이언"들만 할 수 있는 전유물이다. 그래서 손오공의 자식인 손오반, 손오천도 초사이언이 될 수 있다.

![손오공가](\assets\images\java-study\6\손오공가.jpg)

관계로 표현하면 "A is a B"로 표현 하는데, A는 자식이고 B가 부모가 된다. 이것은 [자바스터디 3주차]()에서 공부한 `instanceof`와 같다.



# 자바 상속의 특징

자바 상속의 특징은 다음과 같다.

1. **기존 코드의 재사용**
2. **기존 타입의 확장**

자동차 만드는 것을 생각해보자. 자동차 모델에 따라 선루프, 방탄유리 같은 특별한 요소들을 제공할 수 있지만, 자동차는 공통적으로 엔진, 바퀴가 필요하다. 각각의 자동차들은 모델부터 따로 설계하는 것이 아닌, 기본 디자인을 만들고 확장해가면서 특화된 자동차를 만들어 간다.

마찬가지로 "상속"은 기본 클래스(Super Class)를 만들어 놓고, 상속을 통해 특화된 클래스(Sub Class) 만들어 간다.

>  기본클래스는 부모클래스(Parents Class)라고 불리고, 특화된 클래스를 자식 클래스(Child Class)라고 부른다.

자바 상속은 `class`와 `interface`에 의해 이뤄진다.

## 클래스 상속(Class Inheritance)

클래스 상속에서는 부모 클래스를 상속 받은 자식은 추가 멤버, 매서드를 정의할 수 있다. 클래스를 상속 받을 때는 클래스명 뒤에 `extends` 키워드를 사용하여 상속할 클래스를 작성한다.

**Car**

```java
//Parents Class
public class Car {
    int wheels;
    String model;
    
    void start() {
        System.out.println("자동차 출발!");
    }
}
```

**ArmoredCar(장갑차)**

```java
public class ArmoredCar extends Car {
    int bulletProofWindows; // 새 맴버 변수
    
    void remoteStart() {
        System.out.println("원격 시동");
    }
}
```

`ArmoredCar`는 `Car`의 자식 또는 서브 클래스이다.

```java
public class CarDemo {
    public static void main(String[] args) {
        ArmoredCar armoredCar = new ArmoredCar();
        
        // 부모기능 자식기능
        armoredCar.start();
        armoredCar.remoteStart();
    }
}
```

`armoredCar` 객체는 부모 매서드인 `start()`, 자기 자신의 매서드인 `remoteStart()`모두 사용할 수 있다.

![subclass_inheritance](\assets\images\java-study\6\subclass_inheritance.png)

**클래스 상속의 특징**

1. 새 맴버 변수 및 새 매서드를 선언할 수 있다.
2. Sub Class는 하나의 Class 상속받을 수 있다.(Single Inheritance)

자바는 복잡성을 줄이고 언어를 단순화 하기 위해 다중 상속을 지원하지 않다. 만약 A,B,C 세 클래스가 있을 때, C는 A와 B를 상속 받았을 때, 클래스 A와 B에 같은 이름의 매서드가 있다면, 클래스 A와 B 중 누구의 매서드를 호출한 것인지 정할 수 가 없다. 따라서 `extends`키워드 뒤에 두 개 이상의 클래스를 적을 경우, `compile-error`가 발생하는 것을 볼 수 있다.

**상속 표기**

![Single Inheritance](\assets\images\java-study\6\Single Inheritance.png)

클래스 상속은 위 그림과 같이 다이어그램으로 표현할 수 있다. 화살표 내보내는 클래스가 "자식", 화살표를 받는 클래스가 "부모"가 된다. 그림에서 "클래스B"가 "클래스A"를 참조하고 있는 것이다.

## 인터페이스 상속(Interface Inheritance)

인터페이스는 객체지향 프로그래밍에서 추상화와 관련있다. 자바에서 인터페이스는 클래스처럼 타입으로 사용되지만 객체의 행위를 추상화 해놓은 것으로 매서드 선언만 되어 있고, 상속(implements)받은 클래스는 선언된 매서드를 구현(Override)해야 한다.

**Floatable**

```java
public interface Floatable {
    void floatOnWater();
}
```

 **Flyable**

```java
public interface Flyable {
    void fly();
}
```

**ArmoredCar**

```java
// 장갑차 업그레이드
public class ArmoredCar extends Car implements Floatable, Flyable {
    
    public void floatOnWater() {
        System.out.println("수륙양용");
    }
    
    public void fly() {
        System.out.println("비행가능");
    }
}
```

예제를 보면 `interface`는 `implements`라는 키워드를 통해서 상속된다. 또한 `interface`는 객체를 수식해주는 역할을 하는 것 같다.

**인터페이스 상속 특징**

* 인터페이스 상속은 다중 상속이 가능하다.

그러면 왜 클래스는 다중 상속이 안되는데, 인터페이스는 다중 상속은 가능할까? 클래스의 경우, 메서드가 이미 구현이 되어 있기 때문이다. 그래서 클래스가 다중 상속을 하게 되면 어떤 클래스의 메서드를 가져다 써야하는지 알 수 가 없다. 하지만, 인터페이스는 껍데기만 있을 뿐 내부 로직이 구현이 되어 있지 않기 때문에 다중 상속이 가능하다.

# 메소드 오버라이딩(Method Overriding)

메소드 오버라이딩은 부모 클래스에 정의된 메서드를 "재정의"하는 것을 말한다. 여기서 "재정의"는 매서드를 새로 재정의한다는 개념과 매서드의 기능을 더 넓게 확장한다는 개념을 갖는다.

**Vehicle**

```java
// 부모 클래스
public class Vehicle {

    public String accelerate(long mph) {
        return "vehicle 속력은 : " + mph + " MPH.";
    }

    public String stop() {
        return "vehicle 멈춰!";
    }

    public String run() {
        return "vehicle 달리는 중";
    }
}
```

**Car**

```java
// 자식 클래스
public class Car extends Vehicle {

    String model;

    void start() {
        System.out.println("자동차 출발!");
    }

    @Override
    public String accelerate(long mph) {
        return "자동차 속도 : " + mph + " MPH";
    }
}
```

**Test**

```java
class CarTest {

    private final Vehicle vehicle = new Vehicle();
    private final Vehicle car = new Car();

    @Test
    public void showReference() {
        System.out.println("vehicle = " + vehicle);
        System.out.println("car = " + car);
    }

    @Test
    public void vehicleCallAccelerate() {
        assertThat(vehicle.accelerate(100))
                .isEqualTo("vehicle 속력은 : 100 MPH.");
    }

    @Test
    public void carCallAccelerate() {
        assertThat(car.accelerate(100))
                .isEqualTo("자동차 속도 : 100 MPH");
    }

}
```

다음 테스트 코드를 실행해보면 다음과 같다.

![overriding](\assets\images\java-study\6\overriding.png)

타입은 `Vehicle`이지만, `new Car()`로 생성한 객체는 `Car`객체인것을 볼 수 있다. 그래서 `accelerate`매서드를 실행했을 때, `Vehicle`클래스의 매서드가 아닌 `Overriding`된 매서드가 실행된 것을 볼 수 있다.

# 다이나믹 매서드 디스패치(Dynamic Method Dispatch)

매서드 오버라이딩은 상속을 통해서만 구현될 수 있다는 점에서 보면 compile 타임에 오버라이드 메서드를 실행할지 기본 클래스의 메서드를 실행할지 알 수 없다. 그래서 런타임 시간에 객체의 실행할 메서드를 결정하게 되는 데, 이것을 **Dynamic Method Dispatch** 또는 **Dynamic Binding**이라 한다.

 다이나믹 매서드 디스패치를 통해 **다형성(Polymorphism)**을 지원한다.

# Super

[자바스터디 5주차](https://minsssg.github.io/java/java-study-with-whiteship-05/#this)에서 배운, `this`키워드는 자기 자신의 참조 변수였다. `super`는 자기 부모 클래스를 참조하는 키워드이다.

`super()`는 부모의 생성자를 호출한다. 클래스 상속을 받은 객체는 무조건 부모 생성자를 호출하게 되어 있다.

**BaseClass**

```java
public class BaseClass {
    public BaseClass() {
        System.out.println("Create Base Class");
    }
}
```

**SubClass**

```java
public class SubClass extends BaseClass {
    public SubClass() {
        System.out.println("Create Sub Class");
    }
}
```

![super](\assets\images\java-study\6\super.png)

코드를 실행해보면 `BaseClass`생성자, `SubClass`생성자가 실행된 것을 볼 수 있다. 분명 `SubClass`생성자에는 `BaseClass`생성자를 호출한 적이 없다. 이러한 이유는 상속받은 `SubClass`생성자에 `super()`가 생략되어 있기 때문이다.

또한 `super`는 `this`키워드처럼 부모의 맴버변수를 참조할  수 도 있다.

# 추상 클래스(Abstract Class)

추상 클래스는 구현하지 않은 클래스로 클래스의 공통적인 상태나 기능을 묶어 두는 클래스이다. 이 추상 클래스를 구체적으로 구현한 클래스를 콘크리트 클래스(concrete class)라고 한다.

**작성방법**

```java
public abstract class AbstractClass {
    // ... field, constructor
    public abstract void abstractMethod();
}
```

클래스에 `abstract`키워드를 붙이고 매서드에 `abstract`키워드를 붙인다. 이 클래스를 상속 받으면 `abstract`매서드를 `overriding`해야 한다.

추상클래스는 추상 매서드 뿐만아니라 일반 매서드(concrete method)도 작성할 수 있다. 단, 추상클래스로 객체를 생성할 수 없다.

# final

`final`키워드는 재할당을 막는 키워드이다. `final`키워드를 사용할 수 있는 곳은 다음과 같다.

* class: 클래스에 `final`키워드를 사용하면, 해당 클래스를 상속할 수 없다.
* variable: 변수에 `final`키워드를 사용하면, 재할당 할 수 없다.
* method: 매서드에 `final`키워드를 사용하면, `@Override`할 수 없다.

`final`키워드는 메모리 재할당을 방지하는 것이기 때문에, `Java Collections`객체들의 내부값은 재할당이 가능하다.

```java
public static void main(String[] args) {
    
    final List<Integer> list = new ArrayList<>();
    
    list.add(1);
    list.add(2);
    list.add(3);
}
```

`final`키워드가 붙었지만, `list`객체안에 값추가 가능하다. 이럴 경우 `Collections.unmodifiableList(list)`매서드를 사용하면 값 추가를 막을 수 있다. 또 다른 방법으로는 [일급 객체](https://jojoldu.tistory.com/412)(읽어보자!) 를 사용하는 것이다.

# Object

`Object`는 모든 자바의 최상위 자료형이다. 모든 클래스는 `Object`를 상속받고 있다.

![classes-object](\assets\images\java-study\6\classes-object.gif) 



# 참고자료

* <https://www.geeksforgeeks.org/inheritance-in-java/>
* <https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html>
* <https://www.baeldung.com/java-method-overload-override>
* <https://www.baeldung.com/java-super>
* <https://www.baeldung.com/java-abstract-class>

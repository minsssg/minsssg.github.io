---
title: 자바스터디 8주차
excerpt: "8주차 과제: 인터페이스"
author_profile: true
categories:
  - Java
tags: [Java, Interface]
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2022-01-13T21:00-22:00
header:
  teaser: "/assets/images/java-teaser.jpg"
---

# 개요🙌

**목표:** 자바의 인터페이스에 대해 학습하자.

**학습할 것**

* 인터페이스 정의하는 방법
* 인터페이스 구현하는 방법
* 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법
* 인터페이스 상속
* 인터페이스의 기본 메소드 (Default Method), 자바 8
* 인터페이스의 static 메소드, 자바 8
* 인터페이스의 private 메소드, 자바 9

먼저 인터페이스(interface)를 검색해보면, 위키백과에서 정의를 찾을 수 있다.

> 인터페이스(interface)는 서로 다른 두 개의 시스템, 장치 사이에서 정보나 신호를 주고받는 경우의 접점이나 경계면이다. 즉, 사용자가 기기를 쉽게 동작시키는데 도움을 주는 시스템을 말한다.

인터페이스는 두 개의 장치, 시스템 또는 사물을 연결해주는 것을 말한다. 대표적으로 컴퓨터에 사용되는 **마우스**나 **키보드**가 인터페이스라고 할 수 있다.

인터페이스는 복잡한 시스템을 연결해줌으로써 사용자가 편의를 돕는다. 우리가 마우스, 키보드를 사용하여 쉽게 클릭하고 문자를 작성할 수 있지만, 마우스나 키보드가 어떤 프로세스로 동작하는지 알지 못해도 사용하는데 지장이 없는 것처럼 말이다.

또한, 자판기(Vending Machine)의 버튼도 대표적인 인터페이스이다. 

{% include components/youtubePlayer.html id="2Ug8fVZ9P-M" %}

우리는 자판기에 동전을 넣고 버튼만 누르면 콜라가 나오지만, 안에서 어떤 일이 일어나는지 모르는 것처럼 말이다.

자바에서 인터페이스는 우리가 자판기에 동전을 넣고 안에서 어떤 일이 일어나는지 모르는 것처럼, 클라이언트가 해당 인터페이스를 사용하기만 할 뿐, 내부로직에 의존하지 않게 함으로써 코드를 변경에 유연하게 해준다.

인터페이스는 일종의 추상 클래스(abstract class)이다. 하지만 추상 클래스와 달리 일반 메서드나 맴버변수(지역변수)를 가질 수 없다.(단, 자바 8에서는 Default 메서드를 지원함) 대신, 추상 메서드와 상수 (final static)만 가질 수 있다.

인터페이스는 추상 클래스보다 더 추상화된 형태이다.

# 인터페이스 정의하는 방법

인터페이스는 클래스를 정의하는 것과 같다. 대신 `class`키워드 대신에 `interface`키워드를 사용한다.

접근제어자로는 `public`과 `default`를 사용할 수 있다.

```java
interface <interface-name> {
    public static final <type> <var-name> = <value>;
    public abstract <return-type> <method-name>(<parameters>);
}
```

> 모든 맴버변수는 `public static final`이고, 이는 생략할 수 있다.
>
> 모든 메서드는 `public abstract`이고 이를 생략할 수 있다.
>
> (자바8에서 `static`, `default` 메서드 제외)

```java
interface Car {
    int wheel = 4; // public static final int wheel = 4;
    final int maxSpeed = 300; // public static final int maxSpeed = 300;
    static int cc = 1700; // public static final int cc = 1700;
    
    public abstract int getSpeed();
}
```

# 인터페이스 구현 방법

인터페이스는 추상 클래스처럼 인터페이스 자체로 인스턴스를 생성할 수 없다. 추상 클래스를 상속해서 `concrete` 클래스로 만드는 것처럼 인터페이스는 구현(implements)을 통해 메서드를  작성해야 한다.

클래스는 상속을 통해 클래스를 확장해 나간다는 개념으로 `extends`를 사용하고, 인터페이스는 구현한다는 의미인 `implements` 키워드를 사용한다.

```java
interface VendingMachine {
    Cola getCola(int price);
}

class CocaColaVendingMachine implements VendingMachine {
    
    @Override
    Cola calculate(int price) {
        
        if (price < Cola.price) {
            throw new IllegalStateException("코카 콜라 금액보다 작습니다.");
        }
        
        return new Cola();
    }
}
```

`CocaColaVendingMachine` 클래스는 `VendingMachine` 인터페이스를 구현하여 `getCola()` 메서드를 재정의(override)한 것을 볼 수 있다.

# 인터페이스 레퍼런스를 통해 구현체 사용하는 방법

이제 고객이 콜라 자판기에서 콜라는 출력하는 모습 생각해보자.

```java
class ClientMain {
    public static void main(String[] args) {
        VendingMachine vm = new CocaColaVendingMachine();
        System.out.println(vm.getCola(1000));
    }
}
```

위 코드를 보면 `vm`의 타입은 `VendingMachine`으로 인터페이스 레퍼런스 타입이지만, 실제 `new`키워드로 작성한 것은 `CocaColaVendingMachine` 클래스이다.

따라서, `vm.getCola(1000)` 메서드를 실행하면, `CocaColaVendingMachine`의 `getCola()` 메서드가 실행된다.(이를 다이나믹 바인딩이라고 한다.)

## 인터페이스 상속

인터페이스는 인터페이스로부터만 상속이 가능하고, 다중 상속이 가능하다.

```java
interface Cardable {
    
    void payByCard();
    
}

interface Cashable {
    
    void payByCash();
}
```

`Cardable`, `Cashable`이란 두 인터페이스는 자판기에서 카드로 결제할 수 도 있고, 현금으로도 결제할 수 있는 기능을 만들기 위해 인터페이스를 만들었다.

이 때, `VendingMachine`인터페이스에 상속을 해보자.

```java
interface VendingMachine extends Cardable, Cashable {
    Cola getCola(int price);
}
```

이렇게 자판기는 카드 결제, 현금 결제 기능을 상속받았다. 이제  `VendingMachine`을 구현한 `CocaColaVendingMachine`은 `Cardable`, `Cashable` 인터페이스 추상 메서드를 재정의(Override) 해야 한다.

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

# 자바 8부터 추가된 Default 메서드와 Static 메서드

자바 인터페이스의 내에는 모든 메서드들은 추상 메서드이다. 즉, `body`(내부 구현로직)를 가질 수 가 없이 리턴 타입 및 파라미터에 대한 정의만 한다. 하지만, 자바 8 부터 Default 메서드와 Static 메서드가 추가 되었는데, 이는 인터페이스 내에서 메서드의 `body`를 작성할 수 있게 되었다. 이에 대해 알아보자.

## Default 메서드

일반적으로 부모 클래스에 새로운 메서드를 만드는 것은 어려운 일이 아니다. 하지만, 인터페이스의 경우 새로운 메서드를 만드는 것은 추상 메서드를 선언하는 것이며, 인터페이스를 구현하는 모든 클래스들은 새로운 메서드를 재정의해야 한다.

객체지향적으로 설계를 할 때, 인터페이스는 변경이 되면 안되지만, 아무리 설계를 잘해도 변경이 발생할 수 있다. 그래서 자바 8부터 `Default method`를 통해 인터페이스에 메서드를 추가할 수 있다. => **구현체**를 제공.

만약 자판기에서 일시적으로 리미티드애디션콜라를 판매한다고 할 때, 단순히 `getLimitedEditionCola`라고 작성한다고 할 때, 여러 코드에서 `VendingMachine`인터페이스를 상속받고 있다면, 상속받고 있는 모든 클래스는 `getLimitedEditionCola`를 구현해야 할 것이다. 하지만, `default` 메서드를 통해 `VendingMachine`인터페이스에 바로 구현할 수 있다.

```java
public interface VendingMachine extends Cashable, Cardable{

    Cola getCola(int price);

    default Cola getLimitedEditionCola(int price) {
        return new LimitedEditionCola();
    }
}
```

이렇게 하면, `CocaColaVendingMachine`은 클래스는 따로 변경할 필요가 없다.

> 참고: 인터페이스 다중상속을 할 때, `default`메서드의 이름이 같다면, `default` 메서드를 재정의해야 한다.
>
> 또한, `default`메서드와 부모 클래스의 메서드 간에 이름이 똑같다면, 조상 클래스의 매서드가 상속되고, `default`메서드는 무시된다.

인터페이스 `static` 메서드 역시 `body`를 가질 수 있는데, `default`메서드와 차이점은 바로 상속이 되지 않는다는 점이다.

 # 자바 9부터 추가된 private 메서드

인터페이스 추상 메서드는 접근 제어자 기본적으로 `public`이다. 왜냐하면, 인터페이스를 구현할 때, 해당 추상 메서드를 재정의해야 하기 때문이다.

이전의 추상 클래스 내에 추상 메서드의 접근자가 `private` 이면 컴파일 에러를 내는 것을 볼 수 있는데,`private`으로 하면 상속이 되지 않고, 구현부를 가져야만 하기 때문이다.

이렇게 인터페이스에서 `private`메서드를 지원하게 된 것은 단순히 인터페이스 내부 로직을 처리하는 메서드일 뿐인데 `default`나 `static` 메서드라고 해도 접근제어자가 `public` 이기 때문에 외부에서 접근을 원치 않지만 막을 수가 없었다.

이에 `private`메서드가 추가된 것이다.

# 정리

인터페이스는 사용자가 시스템의 내부 로직을 모르게 함으로써 사용자와 시스템 간의 관계를 유연하게 만들 수 있었다.

또한, `static`, `default`메서드와 자바9에서 `private`메서드가 생긴 이유를 통해서 좀 더 깔끔한 코드를 작성할 수 있다는 것을 알게 되었다.

# 참고자료

* 자바의 정석 3판
* <https://www.notion.so/4b0cf3f6ff7549adb2951e27519fc0e6>


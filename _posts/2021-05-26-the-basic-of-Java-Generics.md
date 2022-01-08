---
title: "자바 Generics이란"
excerpt: "자바 제네릭이란 무엇인가?"
author_profile: true
categories:
  - Java
tags:
  - Java
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2021-05-26T22:00-23:00
header:
  teaser: "/assets/images/java-teaser.jpg"
---

# 1. Generic 이란

자바 제네릭(Generic)은 JDK 5.0(jdk 1.5) 에서 소개되었다. 제네릭은 버그를 줄이는 것을 목표로 한다.

# 2. Generic 이 필요한 이유

`Integer`를 담는 자바 리스트를 만들어보자.

```java
List list  = new LinkedList();
list.add(new Integer(1));
Integer i = list.iterator().next();
```

![nousegenerics](\assets\images\java-study\generics\nousegenerics.png)

`list.iterator().next()`값이 `Object` 객체를 반환하는 것을 볼 수 있다. `list.iterator().next()`는 어떤 값을 반환하는 알 수 없기 때문에, 명시적 타입 캐스팅 (explicit casting)이 필요하다.

```java
Integer i = (Integer) list.iterator.next();
```

![explicit_cast](\assets\images\java-study\generics\explicit_cast.png)

> 에러가 없어진 것을 확인할 수 있음.

`list`에 들어있는 값들을 볼 때, `Object`임을 보장할 수 있으므로, 타입이 안전한지 확인하기 위해 명시적 케스트가 필요하다.

위 코드를 보면 `list`가 `Integer`값이 들어있는 것을 알고 있지만, 값을 사용하기 위해서 명시적 캐스트를 해야한다. 이는 불필요한 코드를 넣게 되고, 개발자가 명시적 캐스트를 잘 못할 경우, `runtimeError`를 발생시킬 수 있다.

그리고, 프로그래머가 특정 타입을 사용하는 **의도를 표현**하고, 컴파일러가 이러한 **타입의 정확성을 보장**하면 위의 문제점을 해결할 수 있다. 이것이 제네릭의 **핵심** 아이디어이다.

```java
// use generic
List<Integer> list = new LinkedList<>();
```

`<>` 다이아몬드 연산자라고 불리는 연산자에 타입을 넣어준다. 이렇게 하면 컴파일러가 컴파일 타임에서 타입을 확인할 수 있다.

# 3. Generic Methods

제네릭 메서드는 하나의 메서드로 다른 유형의 argument와 함께 호출될 수 있는 메서드를 말한다.

컴파일러는 어떤 타입을 사용하더라도 타입 정확성을 보장한다.

제네릭 메서드는 다음과 같은 속성을 가진다.

* 제네릭 메서드는 타입 파라미터를 가진다. (리턴 타입에 다이아몬드 연산자를 말함)
* 타입 파라미터는 범위(bound)를 제한할 수 있다.
* 제네릭 메서드는 콤마(,)로 다른 타입 파라미터를 가질 수 있다. (ex=> <K, V, T, E>)

**제네릭 메서드 사용**

```java
public <T> List<T> fromArrayToList(T[] a) {
    return Arrays.stream(a).collect(Collectors.toList());
}
```

**2개 타입 파라미터 사용**

```java
public static <T,G> List<G> fromArrayToList(T[] a, Function<T,G> mapperFunction) {
    return Arrays.stream(a).map(mapperFunction).collect(Collectors.toList());
}
```

## 3.1 Bounded Generics

타입 파라미터를 제한할 수 있는데, 이것을 **Bounded Generics** 이라 한다. 즉, 타입 파라미터를 원하는 타입으로 제한할 수 있다는 것이다.

* sub classes (upper bound) 상한 - `extends`
* super classes (lower bound) 하한 - `super`

```java
public <T extends Number> List<T> fromArrayToList(T[] a) {
    ...
}
```

다음과 같이 `<T extends Number>`를 사용하게 되면, `Number`클래스의 하위 클래스들만 타입 파라미터로 사용할 수 있다.

![wrapper-classes](\assets\images\java-study\generics\wrapper-classes.png)

`T`에 들어갈 수 있는 클래스는 `Byte, Short, Integer, Long, Float, Double`이다. 다른 클래스들은 `Number`타입의 하위 클래스(subclasses)가 아니기 때문에 사용할 수 없다. (super는 반대로)

## 3.2. Multiple Bounds

타입 파라미터는 두 개 이상의 Bound를 가질 수 있다.

```java
<T extends Number & Comparable>
```

# 4. 와일드 카드

자바에서 와일드 카드는 물음표(?)로 표현된다. 제네릭에서 와일드 카드의 의미는 **알려지지 않은 타입(unknow type)**을 의미한다. 즉, 어떤 타입이 올지 모른다는 것이다.

여기서 와일드 카드의 주의🚧 해야 점을 알려준다.

> `Object`는 자바의 모든 최상위 타입(supertype)이다. 하지만, `Object`의 컬렉션은 컬렉션의 최상위 타입이 아니라는 것이다.

즉, `List<Object>`는 `List<String>`의 상위 타입이 아니라는 것이다.

그래서 다음과 같이 표현하면 컴파일 에러가 발생한다.

```java
//legal
Object obj = new String("Object에 String 객체를 넣을 수 있다.");
// Illegal
List<Object> list = new List<String>();
```

# 5. Type Erasure

자바는 제네릭이 타입 안전성(Type safety)을 보장하고, 런타임에서 오버헤드를 발생시키지 않기 위해 **Type Erasure**를 추가했다. 컴파일러가 컴파일 단계에서 **Type Erasure**를 실행한다.

 Type Erasure는 bounded 타입 파라미터일 경우 해당 클래스로 변경하고, unbounded일 경우, `Object` 클래스 타입으로 변경한다. 따라서 컴파일 후, 바이트코드에는 타입 파라미터 대신 일반 클래스만 존재한다.

```java
// generic method
public <T> List<T> genericMethod(List<T> list) {
    return list.stream().collect(Collectors.toList());
}
```

위의 제네릭 메서드는 `T`라는 타입 파라미터를 가지고 있다. 이 `T`는 unbounded 타입이기 때문에 컴파일 후에는 `Object`파일로 변한다.

```java
public List<Object> genericMethod(List<Object> list) {
    return list.stream().collect(Collectors.toList());
}
```

bounded의 경우

```java
public <T extends Number> void genericMethod(T t) {
    ...
}

/// after compilation
public void genericMethod(Building t) {
    ...
}
```

# 6. Generics과 Primitive 데이터 타입

원시적 타입(Primitive Type)은 제네릭에 사용할 수 없다.

```java
// illegal
List<int> list = new ArrayList<>();
list.add(17);
```

원시적 타입은 왜 제네릭에 쓸 수 없는가? 그것은 **제네릭의 컴파일 타입에서의 특징** 때문이다.

제네릭이 컴파일 타임에서 컴파일 될 때, 타입 파라미터가 **Type Erasure**에 의해 `Object`인 클래스로 변경되기 때문이다. 따라서 원시적 타입은 `Object`로 변할 수 없기 때문에, 타입 파라미터로 사용할 수 없다.

# 7. 결론

자바의 제네릭은 컴파일 에러를 발생시켜 런타임 에러를 줄이는 것이다. 왜냐하면 런타임에러를 고치는 것은 많은 비용(cost)가 발생하기 때문이다. 또한 컴파일 에러가 발생하면 IDE(eclipse, intellij)에서 쉽게 오류를 찾아 프로그램을 실행시키지 않아도 오류를 해결할 수 있기 때문이다.

그리고 type erasure란 기능으로 컴파일 타임에서 타입 파라미터를 변경함으로써 애플리케이션에서 추가적인 오버헤드를 발생시키지 않는다.

다음에는 제네릭에 대한 예제를 가져오고 실무에서 어떻게 쓰이는지 적어보려 한다.

# 8. 출처

* https://www.baeldung.com/java-generics
* Head First Java
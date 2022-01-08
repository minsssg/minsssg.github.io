---
title: "자바스터디 1주차"
excerpt: "JVM은 무엇이며 자바코드는 어떻게 실행되는 것인가?"
author_profile: true
categories:
  - Java
tags: [Java, JVM, JDK, JRE]
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2021-05-22T22:00-23:00
header:
  teaser: "/assets/images/java-teaser.jpg"
---

# 개요🙌

백기선(whiteship)님께서 2020년 11월 8일부터 2021년 3월 17일까지 운영하신 [온라인 자바스터디](https://github.com/whiteship/live-study)가 있었다. 이 때, 한 5주차 쯤에 라이브 방송을 보기 시작했지만 스터디에 참여하지는 않았다. 객체지향 개발자로 성장하고 싶어서 조금 뒷북(?) 이지만 혼자 매주 스터디를 진행보려 한다. 혼자 자료를 찾아서 공부하고 하트💖를 받으신 정말 공부를 잘하시는 분들 내용을 참고하면서 공부하려 한다.

# 1주차: JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가.

**목표**

* 자바 소스 파일(.java)을 JVM으로 실행하는 과정 이해하기.

**학습할 것**

* JVM이란 무엇인가
* 컴파일 하는 방법
* 실행하는 방법
* 바이트코드란 무엇인가
* JIT 컴파일러란 무엇이며 어떻게 동작하는지
* JVM 구성 요소
* JDK와 JRE의 차이

# JVM 이란 무엇인가

> JVM(Java Virtual Machine) 자바 **바이트코드**를 실행할 수 있는 주체이다. 일반적으로 **인터프리터**나 **JIT 컴파일 방식**으로 다른 컴퓨터 위에서 바이트코드를 실행할 수 있도록 구현되나 job  [자바 프로세서](https://ko.wikipedia.org/wiki/자바_프로세서)처럼 하드웨어와 소프트웨어를 혼합해 구현하는 경우도 있다. (이론적으로는 100% 하드웨어 구현도 가능하나 비효율적이다) 자바 바이트코드는 플랫폼에 독립적이며 모든 자바 가상 머신은 자바 가상 머신 규격에 정의된 대로 자바 바이트코드를 실행한다. 따라서 표준 자바 API까지 동일한 동작을 하도록 구현한 상태에서는 이론적으로 모든 자바 프로그램은 CPU나 운영 체제의 종류와 무관하게 동일하게 동작할 것을 보장한다. - 위키백과

솔직히 위키백과 내용은 반만 이해가 된다.

![JvmSpec7](\assets\images\java-study\1\JvmSpec7.png)

출처: [JVM 위키백과](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%EA%B0%80%EC%83%81_%EB%A8%B8%EC%8B%A0)

`Java` 프로그램을 실행하면, 자바 컴파일러가 자바 코드를 `byte code`로 변환한다. `JVM`은 바이트코드를 읽어서 기계어로 변환한다. 

`JVM`은 기본 운영체제 및 시스템 하드웨어에 의존하기 않는 인터페이스를 제공하기 때문에 가상머신이라고 한다.

![getStarted-compiler](\assets\images\java-study\1\getStarted-compiler.gif)

출처: <https://www.javaguides.net/2019/02/java-jvm-jre-jdk-explained-with-diagrams.html>

위선 `JVM`을 간단히 정의하면 *"CPU나 운영체제에 상관없이 바이트코드를 실행하는 주체"*이다. 여기서 **바이트코드**라는 용어가 나오는데 바이트코드가 무엇인지 먼저 알아보자

# 바이트코드란 무엇인가

> 바이트코드(Bytecode, portable code, p-code)는 특정 하드웨어가 아닌 가상 컴퓨터에서 돌아가는 실행 프로그램을 위한 이진 표현법이다. 하드웨어가 아닌 소프트웨어에 의해 처리되기 때문에, 보통 기계어보다 더 추상적이다. - 위키백과

바이트코드의 위키백과를 보면, *"가상 컴퓨터에서 돌아가는 실행 프로그램을 위한 이진 표현법"*이라고 되어 있다. JVM이 Java Virtual Machine, 즉, 가상 컴퓨터이기 때문에 바이트코드는 JVM에서 실행되는 언어이다.

바이트코드는 컴파일러에 의해 생성된다. 자바 언어도 컴파일을 통해서 바이트코드를 만든다. 그럼 이제 컴파일하는 방법을 확인해보자.

# 컴파일하는 방법

1. 자바파일(.java)을 생성한다.
2. 자바컴파일러(javac)로 컴파일한다.

간단히 프로그래밍 시작 국룰(?) ```Hello World```를 찍는 자바 파일(.java)파일 생성한다.

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

`javac` 자바 컴파일러로 `HelloWorld.java`파일을 컴파일 해보자

```bash
javac HelloWorld.java
```

![자바컴파일](\assets\images\java-study\1\java-compile.PNG)

컴파일 결과 ```HelloWorld.class```가 생성된 것을 볼 수 있다. 자바에서는 `.class`이 **바이트코드**이다.

또한, 생성된 바이트 코드를 확인하는 방법도 있다. `javap` 명령에 옵션 `-c`를 붙이면 바이트코드를 역어셈블하게 코드를 확인해 볼 수 있다.

```bash
javap -c HelloWorld
```

![자바역컴파일](\assets\images\java-study\1\java-decompile.PNG)

다음으로 바이트코드를 실행하는 방법을 알아보자.

# 실행하는 방법

터미널 창에서 `java`명령어를 통해서 `*.class`파일을 실행할 수 있다.

```bash
java HelloWorld
```

![자바실행](\assets\images\java-study\1\java-run.PNG)

**자바 프로그램 실행 과정**

위의 명령을 실행하게 되면 바이트코드를 JVM에서 실행하게 되고 프로그램에서 사용할만큼 메모리가 할당되고 이를 JVM에서 관리한다.

# JIT 컴파일러란 무엇이며 어떻게 동작하는지

처음 [JVM 이란 무엇인가](#1) 에서 위키백과의 JVM이 설명에 **JIT 컴파일러**에 대한 설명이 나온다.

> JIT 컴파일(Just-In-Time compilation) 또는 동적 번역(dynamic translation)은 프로그램을 실제 실행하는 시점에 기계어로 번역하는 컴파일 기법이다.
>
> ...
>
> 자바 컴파일러가 자바 프로그램 코드를 바이트코드로 변환한 다음, 실제 바이트코드를 실행하는 시점에서 자바 가상 머신이 바이트코드를  JIT 컴파일을 통해 기계어로 변환한다.

우선 자바코드를 `javac`로 컴파일하면 바이트코드가 생성되는데 바이트코드를 JVM에서 실행할 때, JIT 컴파일러에 의해 기계어로 변환된다는 것이다.

즉, JIT 컴파일러는 `javac`에 의해 변환된 바이트코드를 기계어로 변환하는 컴파일러이다.

바이트코드는 CPU가 읽을 수 있는 형태가 아니다. 그래서 CPU가 읽을 수 있도록 컴파일해야 하는 것이다.

이렇게 두 번 컴파일하는 이유는 **machine-independent** 을 위한 것이라고 한다. 이렇게 하면 어떤 cpu, os에서 실행할 수 있다는 것이다.

![jit컴파일러](\assets\images\java-study\1\jit-compiler.png)

출처: https://bloofer.tistory.com/21

# JVM 구성 요소😵

![JVM구성요소](\assets\images\java-study\1\JVM-configuration.png)

* **Class Loader**

Class Loader는 런타임 중에 Java 클래스를 JVM에 동적으로 클래스 파일을 로드하는 역할을 한다. 또한 클래스 파일을 한번에 로드하지 않고 응용 프로그램이 필요할 때 로드한다.

* **Execution Engine**

Execution Engine은 바이트코드를 실행한다. 또한 GC(Garbage Collector)가 Execution Engine에 포함된다. GC는 사용하지 않는 인스턴스를 삭제하는 역할을 수행한다.

# JDK와 JRE의 차이

![Java JDK JRE and JVM](\assets\images\java-study\1\Java JDK JRE and JVM.png)

출처: <https://www.javaguides.net/2019/02/java-jvm-jre-jdk-explained-with-diagrams.html>

## JRE(Java Runtime Environment)

JRE(Java Runtime Environment)는 java 애플리케이션을 실행하는 데 사용되는 소프트웨어 구성 요소의 번들이다.

**JRE 핵심 요소**

* JVM 구현체
* 자바 프로그램 실행하는데 필요한 클래스 파일
* 설정 파일

$$ JRE = JVM + Java Packages Classes (util, math, lang, awt, swing etc) + runtime libraries $$

## JDK(Java Development Kit)

JDK(Java Development Kit)은 자바 프로그램 개발, 디버깅, 컴파일을 실행하기 위한 환경 및 도구를 제공한다.

**JDK 핵심 요소**

* JRE
* Development Tools(Java compiler, debugger, JShell etc)

## 둘의 차이

결국 JDK안에 JRE가 포함되는 구조이다. 또한 JVM은 JRE 안에 포함되어 있는 구조이다. 그래서 요즘 JDK만 다운받으면 JRE가 포함되어 있다.

만약 `Java` 프로그램을 실행만 하고 싶다면, `JRE`만 있으면 된다. 하지만 개발 또는 컴파일을 해야 한다면 `JDK`가 있어야 한다.

# 마무리

5월 20일부터 22일까지 검색과 이미 작성한 글들을 보고 1주차 스터디를 작성해 보았다. 평소에 그냥 당연시 쓰던 것들이 어떻게 구성되어 있고 어떻게 동작하는 알게 되어 좋은 공부였던 것 같다.

하지만, 아직 모두 이해가 되진 않는다. 아직 JVM 구성 요소는 너무 방대한 어떤 동작을 하는지 완벽히 이해하진 못했지만 추후에 부족한 내용을 매꾸려 한다.

# 참고자료

* [JIT 컴파일러(Compiler)에 대해 알아본다.](https://aroundck.tistory.com/1949)
* [Java -JIT 컴파일러](https://medium.com/@ahn428/java-jit-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EB%9F%AC-c7d068e29f45)
* https://www.baeldung.com/jvm-vs-jre-vs-jdk
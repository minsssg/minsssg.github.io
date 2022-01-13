---
title: 자바스터디 7주차
excerpt: "7주차 과제: 패키지"
author_profile: true
categories:
  - Java
tags: [Java, Package, import, classpath, access provider]
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2022-01-13T21:00-22:00
header:
  teaser: "/assets/images/java-teaser.jpg"
---

# 개요🙌

**목표:** 자바의 패키지에 대해 학습하자.

**학습할 것**

* package 키워드
* import 키워드
* 클래스 패스
* CLASSPATH 환경변수
* -classpath 옵션
* 접근지시자

패키지(package)란, `class`, `interface`, `Enumeration`등을 사용 용도나 특징에 맞게 묶어 놓을 것을 말한다. 클래스 하나 당 물리적으로 하나의 파일인 것처럼 패키지는 하나의 디렉토리이다.

# package 키워드

`package` 키워드는 자바소스파일(.java)에 첫 번째 문장에 위치한다.

`package`는 클래스 파일을 담고 있는 디렉토리이므로, `package` 키워드 뒤에 경로(Path)를 작성한다. 이 때, `package` 경로에 점(.)으로 계층구조를 표현할 수 있다.

```java
package 패키지명;
```

대표적으로 `String`클래스는 `java.lang` 패키지를 가지는 데, 다음 사진과 같이 해당 경로를 볼 수 있다.

![java-lang-string](\assets\images\java-study\7\java-lang-string.png)

## 네이밍 Convention

패키지명은 대소문자를 허용한다. 하지만, 클래스명과 쉽게 구분하기 위해 `package`명은 소문자로만 쓴다.

## 이름없는 패키지(Unnamed Package)

모든 클래스는 반드시 하나의 패키지에 속해야 해서, 자바소스파일 시작은 `package`키워드로 시작한다. 하지만, `package`키워드가 없이 자바파일을 작성할 수 있는데 이렇게 `package`키워드가 없는 경우, 자바에서 '이름 없는 패키지(Unnamed Package)' 로 제공한다.

'이름 없는 패키지'는 오로지 '이름 없는 패키지' 끼리 참조가 가능하며, `package`가 명시된 자바소스파일에선 '이름 없는 패키지'를 참조할 수 없다.

## 클래스 참조

클래스 참조는 `package`의 경로를 통해서 참조가 가능하다.

```java
package com.javastudy.javapackage;

public class ExampleClass {
    String className = "ExampleClass";
}
```

다음과 같이 `ExampleClass` 클래스가 `com.javastudy.javapackage` 패키지를 가지고 있다.

이를 다른 클래스에서 참조하는 방법은 다음과 같다.

```java
package com.javastudy;

public class JavaStudyMain {

    public static void main(String[] args) {

        com.javastudy.javapackage.ExampleClass exampleClass = new com.javastudy.javapackage.ExampleClass();
    }
}
```

`com.javastudy.javapackage.ExampleClass` 와 같이 클래스를 참조하려면 패키지 경로를 포함하여 작성해야 참조할 수 있다. 이렇게 참조하는 방법을 `FQCN`(Full Qualified Class Name)이라 한다.

하지만, `String`또는 `Wrapper`클래스들은 `java.lang`패키지에 속하는데, `java.lang`에 포함된 클래스들은 따로 패키지 경로를 작성하지 않고 클래스명만 작성해도 사용할 수 있다.

# import 키워드

위 코드를 보면 `ExampleClass` 클래스를 참조하기 위해 `com.javastudy.javapackage` 패키지 경로까지 작성하고 `new` 키워드 이후에도 이 코드를 작성했다. 딱 봐도 너무 길고 가독성이 좋지 않다. `import`키워드를 사용하면 코드의 가독성을 높일 수 있다.

## import문 선언

일반적인 자바 파일 구성

1. package문
2. import 문
3. 클래스 선언문

`import`키워드는 `package`키워드 밑에 작성하며, `import`문 이후에 사용할 클래스의 패키지명을 작성하여 클래스 참조 시 패키지 경로를 생략할 수 있다.

```java
package com.javastudy;

import com.javastudy.javapackage.ExampleClass;

public class JavaStudyMain {

    public static void main(String[] args) {

        ExampleClass exampleClass = new ExampleClass();

    }
}
```

`import com.javastudy.javapackage.Example;`을 선언하므로써 `ExampleClass`를 참조하겠다는 것을 명시한 것이다. 그래서 main문에서 패키지 경로 없이 `ExampleClass`클래스를 참조할 수 있다.

import 할 때, `import com.javastudy.javapackage.*` 이런 식으로 아스테리크(*)를 붙이면 해당 패키지의 모든 클래스를 참조할 수 있어서 편하다. 하지만, 패키지 수가 많을 때 클래스가 어느 패키지에 속하는지 찾기 어렵지만, IDE의 도움을 받으면 쉽게 확인가능하다.

> 참고: import문에 아스테리크(*) 붙이면 해당 패키지 내에 있는 모든 클래스를 참조할 수 있다. 하지만, 이렇게 선언하면 성능에 안좋은 영향을 미치는 줄 알았다. 하지만, 컴파일 시간이 아주 조금 더 걸릴 뿐 성능에 영향을 미치지 않는다고 한다.

 ## static import

`static import`는 해당 클래스의 `static`맴버(변수 또는 메서드)를 호출할 때, 클래스 명을 생략할 수 있다.

* 그냥 import : 패키지 명 생략
* static import : 클래스 명 생략

> 실제 구문은 `import static `으로 적지만, 이를 명명할 땐, `static import`라고 부른다.

**System.out**

```java
import static java.lang.System.out;

public StaticImportDemo {
    public static void main(String[]) {
        out.println("static import");
    }
}
```

코드를 간결하게 쓸 수 있다.

# 클래스 패스

클래스 패스(classpath)는 말 그대로 클래스가 있는 경로를 말한다. 클래스패스는 `JVM`이 모든 파일을 뒤지는 것이 아니라, `CLASSPATH`변수를 통해서 원하는 위치를 찾는다.

## CLASSPATH 환경 변수

환경변수는 운영체제에서 지정하는 변수로 `JVM`과 같은 애플리케이션들이 참조하는 변수같은 것이다.

`Windows` 운영체제에서는 "시스템 환경 변수 편집"에서 "고급"탭의 "환경 변수"를 클릭하면, "환경 변수" 팝업 창에서 `CLASSPATH`환경변수를 확인할 수 있다. 그리고 이 `CLASSPATH` 변수로 부터 `JVM`의 클래스 로더는 이 환경 변수를 호출하여 해당 위치에 있는 클래스들을 등록한다.

```bash
SET CLASSPATH=.;C:\java\oracle\ojdbc6\ojdbc6-11.2.0.4.jar;
```

다음처럼 `CMD`에서 실행하면, 오라클 jdbc의 클래스를 사용할 수 있다.

# -classpath 옵션

 `-classpath` 또는 `-cp`옵션은 CMD나 터미널 명령창에서 `Java` 파일을 컴파일하거나, 실행할 때 일시적으로 클래스 패스를 설정해 주는 것이다.

```bash
javac -cp .;C:\java\oracle\ojdbc6\ojdbc6-11.2.0.4.jar; com.javastudy.ClassPathDemo.java

java -cp .;C:\java\oracle\ojdbc6\ojdbc6-11.2.0.4.jar; com.javastudy.ClassPathDemo
```

# 접근 지시자 (Access Modifier)

`Access Modifier`한국말로 접근 지시자 또는 접근 제어자라고 한다. 접근 제어자는 해당 클래스 또는 맴버(변수, 메서드)가 외부에서 접근 즉, 참조하도록 할 것인지 하지 않을 것인지를 설정한다.

`Java`에서 제공하는 접근 제어자는 다음과 같다.

* **private**: 같은 클래스 내에서만 접근 가능하다.
* **default**: 같은 패키지 내에서만 접근 가능하다.
* **protected**: 같은 패키지 내에서, 그리고 다른 패키지의 서브 클래스(상속)에서 접근 가능하다.
* **public**: 모든 곳에서 접근 가능하다.

> public > prortect > (default )> private
>
> 참고: 모든 클래스 및 맴버는 접근 제어자를 가진다. 그런데 클래스나 맴버 변수 앞에 접근 제어가 붙지 않는 것을 볼 수 있는데, 이는 `default`접근 제어자가 생략 되어 있기 때문이다.

**대상별 접근 제어자**

* 클래스 : public, default
* 메서드 + 맴버변수 or 생성자: public, protected, default, private
* 지역변수: 사용할 수 없음.

## 접근 제어자를 사용하는 이유

1. 외부로부터 데이터를 보호하기 위해(캡슐화)
2. 접근 제한: 내부적으로만 클래스를 사용할 때, 접근제어자를 통해 패키지를 제한한다.
3. 싱글톤 객체를 만들 때, 접근 제어자 사용

```java
class Singleton {
    
    public static Singleton singleton = new Singleton(); // 클래스 내부에선 접근 가능.
    
    private Singleton() {
        //...
    }
}
```

생성자 앞에 `private` 접근 제어자로 지정하면 `Singleton` 클래스 외부에서 `new`키워드로 객체를 생성할 수 없다. 그래서 `public static`구문으로 외부에서 객체를 참조하게 한다.

# 정리

이번 과제를 하면서, "패키지"의 의미, 패키지를 써야하는 이유를 배웠다.

* 클래스 이름의 충돌을 막고, 패키지를 묶으므로써 관리의 용의성이 있다.
* 접근 제어자를 통해 참조가능한 패키지 범위를 제한함으로써 데이터 접근을 제어한다.

# 참고자료

* 자바의 정석 3판
* <https://www.geeksforgeeks.org/classpath-in-java/>
* <https://usemynotes.com/packages-in-java/>
* <https://kils-log-of-develop.tistory.com/430?category=923003>


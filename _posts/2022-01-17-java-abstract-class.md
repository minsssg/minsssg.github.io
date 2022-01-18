---
title: Java 추상 클래스(abstract class)
excerpt: "Java 추상 클래스(abstract class)에 대해 알아보자."
author_profile: true
categories:
  - Java
tags: [Java, abstract]
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2022-01-17T21:00-00:00
header:
  teaser: "/assets/images/java-teaser.jpg"
---

# 개요🙌

자바 스터디 7주차 및 자바 제어자(Modifier) 공부 중 `abstract`클래스에 대해 간략히 설명했다. 오늘은 `abstract`클래스에 대해 조금 더 알아보고 내용을 정리하고자 한다.

<mark>추상클래스는 "미완성된 설계도"로 많이 비유한다. 추상클래스는 "추상화(abstraction)"를 통해서 만들어지는데, "추상화"란 클래스간의 공통점을 찾아낸 것이다. 이러한 추상클래스는 새로운 클래스를 작성하는데 바탕이 된다.</mark>

`abstract` 단어는 "추상의", "미완성의" 란 의미를 지니고 있다. `abstract` 제어자는 클래스와 메서드 앞에 사용할 수 있다. 그리고 클래스에 `abstract`가 붙으면 `abstract class`이고, 메서드에 붙게 되면 `abstract method`가 된다. 또한, 클래스 내에 `abstract method`가 하나라도 존재하면, `abstract class`가 된다.

`abstract class`는 객체를 생성할 수 없다.

> 참고: 클래스 내에 `abstract method`가 없어도 클래스 앞에 `abstract`제어자를 사용할 수 있다. 또한, `private` 접근 제어자를 사용할 수 없다.

# Abstract class 특징

* 클래스 앞에 `abstract` 제어자가 온다.
* `abstract`클래스는 sub class를 가져야 한다. (상속을 해서 사용해야 한다.)
* `abstract`매서드가 하나라도 있으면, `abstract` 클래스이다.
* `abstract`클래스는 `abstract`매서드와 `concrete`매서드 둘 다 정의할 수 있다.
* 추상 클래스를 상속받은 하위 클래스들은 모든 추상 메서드를  구현하거나, 추상 클래스를 다시 추상 클래스로 상속 받아야 한다.(추상 클래스를 다시 추상 클래스로 상속할 수 있다.)

**BoardGame**

```java
public abstract class BoardGame {
    
    String className = "BoardGame";
    
    // 추상 메서드 : 메서드를 정의하지 않음.
    public abstract void play();
    
    public abstract void showRule();
}
```

**HalliGalli**

```java
public class HalliGalli extends BoardGame {

    String className = "HalliGalli";

    @Override
    public void play() {
        System.out.println("let's go play HalliGalli");
    }

    @Override
    public void showRule() {
        System.out.println("똑같은 그림이 5개가 나오면 종을 친다.");
    }
}
```

# 추상 클래스를 사용하는 시기

일반 클래스보다 추상 클래스를 사용해야 하는 몇가지 시나리오를 분석해보자.

* 하위 클래스들이 공통적으로 수행하는 메서드가 있을 때.(위 예제에서의 `play()`, `showRule()`과 같은 매서드)
* 하위 클래스가 변경이 쉽도록 부분적으로 API를 정의해야 한다.
* 하위 클래스는 하나 이상의 메서드를 상속 받는데, `protected` 접근 제어자를 사용해야 한다.

추상 클래스의 예제를 살펴보자.

`BaseFileReader`는 파일을 읽어오는 클래스이다. `readFile()`은 추상 메서드로서, `BaseFileReader`클래스를 상속받아 `readFile()`을 재정의함으로써, 문자열을 소문자 또는 대문자 등으로 변경할 수 있다.

**BaseFileReader**

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.List;
import java.util.stream.Collectors;

public abstract class BaseFileReader {

    protected Path filePath;

    protected BaseFileReader(Path filePath) {
        this.filePath = filePath;
    }

    public Path getFilePath() {
        return filePath;
    }

    public List<String> readFile() throws IOException {
        return Files.lines(filePath)
                .map(this::mapFileLine).collect(Collectors.toList());
    }

    // 추상 메서드
    protected abstract String mapFileLine(String line);
}

```

`BaseFileReader` 추상 클래스에 `filePath` 맴버변수는 `protected` 접근 제어자로 선언되어 하위 클래스만 `filePath`에 접근할 수 있게 했다.

`mapFileLine()` 추상 메서드는 파일의 내용을 어떤게 읽은 것인지 껍데기만 만들어놓았다. 이 메서드를 상속받아서 "소문자", "대문자"로 만들어 보자.

**LowercaseFileReader**

```java
public class LowercaseFileReader {
	
    public LowercaseFileReader(Path filePath) {
        super(filePath);
    }
    
    @Override
    protected String mapFileLine(String line) {
        // String문자열을 소문자로 만든다.
        return line.toLowerCase();
    }
}
```

**UppercaseFileReader**

```java
public class UppercaseFileReader {
    
    public UppercaseFileReader(Path filePath) {
        super(filePath);
    }
    
    @Override
    protected String mapFileLine(String line) {
        // String 문자열을 대문자로 변경
        return line.toUpperCase();
    }
}
```

**Application**

```java
import com.javastudy.abstractclasses.BaseFileReader;
import com.javastudy.abstractclasses.LowercaseFileReader;
import com.javastudy.abstractclasses.UppercaseFileReader;

import java.io.IOException;
import java.net.URISyntaxException;
import java.nio.file.Path;
import java.nio.file.Paths;

public class Application {

    public static void main(String[] args) throws URISyntaxException, IOException {

        Application application = new Application();

        Path path = application.getPathFromResourcesFile("files/test.txt");

        BaseFileReader lowercaseFileReader = new LowercaseFileReader(path);
        lowercaseFileReader.readFile().forEach(System.out::println);

        BaseFileReader uppercaseFileReader = new UppercaseFileReader(path);
        uppercaseFileReader.readFile().forEach(System.out::println);
    }

    private Path getPathFromResourcesFile(String filePath) throws URISyntaxException {
        return Paths.get(getClass().getClassLoader().getResource(filePath).toURI());
    }
}
```

**files/test.txt**

```txt
This is test file
You Can SEE THIS.
```

**결과**

```
this is test file
you can see this.
THIS IS TEST FILE
YOU CAN SEE THIS.
```

![abstract-class-inherit](\assets\images\java-study\abstract-class\abstract-class-inherit.png)

`Application` 의 `main()`메서드 내에서 `lowercaseFileReader`변수가 `readFile()` 메서드를 실행할 때, `BaseFileReader`의 `readFile()` 메서드를 실행하고, `readFile()` 메서드 정의를 보면, `mapFileLine()` 메서드를 실행하고 있는데 이 때, `mapFileLine()` 메서드는 `LowercaseFileReader`클래스에서 재정의한 메서드가 실행된다.

# 정리

오늘은 추상클래스의 특징과 추상클래스 사용 시기에 대해 배웠다.

위 예제의 `BaseFileReader` 추상 클래스를 상속함으로써, 다른 기능의 파일 입력기를 손쉽게 만들 수 있다. 이렇게 상속을 통해 기능을 확장하고, 부모 클래스나 다른 서브 클래스에 영향을 주지 않는 것을 객체 지향 원칙 중 OCP(Open Closed Principle)이라 한다. 또한 객체 지향의 특징 중 하나인 "추상화(Abstraction)"와 관련 있다.

추후에 객체 지향 원칙인 SOLID과 객체 지향 특징에 대한 글도 정리하려 한다.

# 참고자료

* 자바의 정석 3판

* <https://www.baeldung.com/java-abstract-class>

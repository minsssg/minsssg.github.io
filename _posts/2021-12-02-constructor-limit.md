---
title: "객체 생성자 제한하기"
excerpt: "new 키워드 대신 static 매서드를 쓰자."
author_profile: true
categories:
  - Java
tags:
  - Java
  - 리펙토링
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2021-12-01T12:00-13:00
header:
  teaser: "/assets/images/java-teaser.jpg"
---

# 객체 생성자 제한하기

여러 사람과 개발할 일이 없고, 또한 클래스를 서로 공유하는 일이 없는 나로써, 객체를 생성자를 `public`으로 사용한다. 그런데 만약 여러 사람과 공유 클래스를 개발할 경우, 클래스 필드가 여러 개이고, 필드를 수정할 때 생성자를 `public`으로 하는 것보다 `protected`, `private` 접근자로 생성자를 만들어 `new`키워드로의 객체 생성을 막고 객체 생성메서드를 따로 만드는 것이 유지보수에 좋다.



예를 들어 보자.

```java
@Setter @Getter
public class Book {
    
    private String title; // 제목
    private String from;  // 출판사
    private String price;  // 가격
    
    public Book() {  
    }
    
    public Book(String title, String from, String price) {
        this.title = title;
        this.from = from;
        this.price = price;
    }
}
```

책을 주문하는 서비스1, 서비스2가 있다고 할자.

**서비스1**

```java
public class BookService1 {
    
    public void orderBook() {
        ...
        Book book = new Book(title, from, price);
        ...
    }
}
```

**서비스2**

```java
public class BookService2 {
    
    public void orderBook() {
        ...
        Book book = new Book();
        book.setTitle(title);
        book.setFrom(from);
        book.setPrice(price);
        ...
    }
}
```

서비스1, 서비스2를 보면 둘 다 `Book`객체를 생성하는 로직이다. 이렇게 되면 나중에 필드가 추가되거나 생성 로직이 변경되었을 때, 유지보수에 어려움을 겪는다.

그래서 `new`키워드를 제한하고, `static`메서드를 통해 생성메서드를 만든다.

**Book 리펙토링**

```java
@Getter @Setter
public class Book {
    private String title; // 제목
    private String from;  // 출판사
    private String price;  // 가격
    
    protected Book() {}
    
    public static createBook(String title, String from, String price) {
    	Book book = new Book();
        book.setTitle(title);
        book.setFrom(from);
        book.setPrice(price);
    }
}
```

이제 `Book`객체를 생성하려면 `createBook()`메서드를 사용해야 한다.

```java
public class BookService {
    public void order() {
        ...
        Book book = Book.createBook(title, from, price);
        ...
            
        Book book = new Book();
    }
}
```

`BookService`가 `Book`과 다른 패키지라면 `new`키워드로 객체를 생성할 수 없기 때문에 객체 생성메서드로 제한을 한다.

# 정리

1. 객체를 생성할 때, 생성 메서드를 통해 제한을 하자.
2. 제한하는 이유는 유지보수를 높이기 위함.


---
title: "JAVA 객체 정렬하기"
excerpt: "JAVA 객체를 정렬해보자!"
author_profile: true
categories:
  - Java
  - Algorithm
tags:
  - Java
  - Algorithm
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2020-11-11T13:00-18:00
---

## 1. 개요

JAVA로 개발하거나 알고리즘 문제를 풀다보면 객체를 정렬해야 할 때가 있습니다. 어떻게 단순히 정렬알고리즘을 직접 구현해서 할 수 도 있지만, 이미 ```sort```라이브러리가 잘 구현되어 있기 때문에 라이브러리를 잘 사용하는 방법이 중요한 것 같습니다.

## 2. Comparable vs Comparator

JAVA 객체를 정렬하는 데 두 가지 ```interface```를 사용하여 정렬합니다.

### 2-1. Comparable

```Comparable```은 클래스를 ```implements``` 할 때 ```compareTo()``` 메서드를 ```@Override```를 통해서 구현합니다. 즉,  ```Comparable``` 인터페이스를 통해 객체를 정렬할 수 있는 객체로 만드는 것 입니다.

```compareTo()``` 함수는 ```int``` 를 반환하는 데, 리턴값에 따라 정렬을 합니다.

* 음수 리턴: 오름차순 정렬
* 양수 리턴: 내림차순 정렬

가끔씩 하나로 먼저 정렬을 하고 이후 다른 값으로 정렬해야 할 경우가 있습니다. 예를 들어 학교 성적 같은 경우가 해당합니다.  성적으로 내림차순하고 이름을 오름차순하는 예제를 보이겠습니다.

```java
import java.util.List;
import java.util.ArrayList;
import java.util.Collections;

class Student implements Comparable<Student> {
    
    private String name;
    private int score;
    
    public Student(String name, int score) {
        this.name = name;
        this.score = score;
    }
    
    @Override
    public String toString() {
        return this.name + ": " + this.score;
    }
    
    @Override
    public int compareTo(Student other) {
        // 점수로 내림차순
        if (this.score < other.score) {
            return 1;
        } else if (this.score == other.score) {
            // 점수가 같을 땐 이름으로 오름차순
            if (this.name.compareTo(other.name) > 0) {
                return 1;
            }
        }
        
        return -1;
    }
}

public class ComparableExample {
    public static void main(String[] args) {
        
        List<Student> studentList = new ArrayList<Student>();
        
        studentList.add(new Student("Kim", 100));
        studentList.add(new Student("Lee", 95));
        studentList.add(new Student("Son", 100));
        studentList.add(new Student("Park", 90));
        studentList.add(new Student("Jin", 90));
        studentList.add(new Student("Choi", 95));
        studentList.add(new Student("Kang", 90));
        
        Collections.sort(studentList);
        
        System.out.println(studentList.toString());
    }
}
```

### 2-2. Comparator

```Comparator``` 는 정렬 가능한 클래스 **기본 정렬 기준과 다르게** 정렬하고 싶을 때 사용하는 인터페이스입니다. 익명클래스로 활용되고 정렬할 수 있는 클래스를 내림차순으로 만들 때 사용합니다.

```Comparator``` 는 ```implements``` 후 ```compare()```메서드를 ```@Override```하여 구현합니다.

```compare()``` 메서드 역시 ```return```값으로 ```int```를 반환합니다.

*  a < b : 음수 리턴
* a == b : 0 리턴
* a > b : 양수 리턴

```a < b```에서 음수를 리턴하는 것으로 음수를 리턴하면 오름차순 정렬이 되는 것을 알 수 있습니다.

```return```을 다음처럼 작성할 수 있다.

```return (x < y) ? -1 : ((x==y) ? 0 : 1);```

``` java
import java.util.List;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;

class Student implements Comparable<Student> {
    
    private String name;
    private int score;
    
    public Student(String name, int score) {
        this.name = name;
        this.score = score;
    }
    
    public String getName() {
        return this.name;
    }
    
    public int getScore() {
        return this.score;
    }
    
    @Override
    public String toString() {
        return this.name + ": " + this.score;
    }
    
    @Override
    public int compareTo(Student other) {
        // 단순히 이름 순으로 정렬
        return this.name.compareTo(other.name);
    }
}

class StudentComparator implements Comparator<Student> {
    @Override
   	public int compare(Student x, Student y) {
        if (x.getScore() < y.getScore()) {
            return 1;
        } else if (x.getScore() == y.getScore()) {
            return x.getName.compareTo(y.getName());
        }
        
        return -1;
    }
}

public class ComparableExample {
    public static void main(String[] args) {
        
        List<Student> studentList = new ArrayList<Student>();
        
        studentList.add(new Student("Kim", 100));
        studentList.add(new Student("Lee", 95));
        studentList.add(new Student("Son", 100));
        studentList.add(new Student("Park", 90));
        studentList.add(new Student("Jin", 90));
        studentList.add(new Student("Choi", 95));
        studentList.add(new Student("Kang", 90));
        
        // 정렬 기준을 새로 정의
        StudentComparator sComparator = new StudentComparator();
        // 새로운 정렬기준으로 정렬
        Collections.sort(studentList, sComparator);
        
        System.out.println(studentList.toString());
    }
}
```

## 3. Comparable 과 Comparator의 차이점

```Comparable```과 ```Comparator```는 정렬 기준을 작성하는 데 똑같은 기능을 합니다.

*  ```Comparable```은 기본적으로 객체를 정렬가능하게 만들어줍니다.
* ```Comparator```는  ```Comparable```한 객체에 사용자가 원하는 정렬을 할 수 있도록 해줍니다.

이러한 두 가지의 차이점에 대해 알아보았습니다.

글에 대한 오타 및 오류 신고 항상 감사합니다.

감사합니다.
---
title: "JAVA 객체 정렬하기"
excerpt: "JAVA 객체를 정렬해보자!"
author_profile: true
categories:
  - JAVA
  - Algorithm
tags:
  - JAVA
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

#### 2-1. Comparable

Comparable은 클래스를 ```implements``` 할 때 ```compareTo()``` 메서드를 ```@Override```를 통해서 구현합니다.

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
        if (this.score > other.score) {
            return 1;
        } else if (this.score == other.score) {
            // 점수가 같을 땐 이름으로 오름차순
            if (this.name.compareTo(other.name) < 0) {
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




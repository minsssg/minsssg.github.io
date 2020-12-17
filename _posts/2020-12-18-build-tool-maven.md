---
title: "MAVEN 공부"
excerpt: "MAVEN 공부를 해보자!"
author_profile: true
categories:
  - MAVEN
tags:
  - MAVEN
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2020-12-18T13:00-18:00
---

## 1. 개요

`MAVEN`이란?

>아파치 메이븐(Apache Maven)은 자바용 프로젝트 관리 도구이다. 아파치 앤트의 대안으로 만들어졌따. 아파치 라이선스로 배포되는 오픈 소스 소프트웨어이다. -위키백과-

메이븐은 자바 프로젝트 관리 도구라고 하고 Build 도구라고 합니다. 빌드도구는 프로젝트 생성부터 코드작성, 컴파일, 테스트, 배포까지 빌드라고 합니다. 이를 도와주는 것을 빌드도구라고 합니다.

메이븐 설치는 maven홈페이지에서 확인하실 수 있습니다.

## 2. Maven 실행해보기

Maven 홈페이지에 접속해보면 ```Maven in 5 Minutes```라는 포스트를 볼 수 있습니다.

프로젝트 생성하는 글이 나와 있습니다.

```bash
mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
```

mvn 명령을 작성하고 ```-D```가 구분자 입니다.

archetype:generate가 프로젝트를 만들겠다는 뜻이고, artifactId로 프로젝트 이름을 정하는 것 같습니다. 기타 옵션은 더 공부해야 될 것 같습니다.

이렇게 생성된 프로젝트의 구조를 확인해보면 다음과 같습니다.

```bash
my-app
|-- pom.xml
`-- src
    |-- main
    |   `-- java
    |       `-- com
    |           `-- mycompany
    |               `-- app
    |                   `-- App.java
    `-- test
        `-- java
            `-- com
                `-- mycompany
                    `-- app
                        `-- AppTest.java
```

## 3. 마무리

오늘은 maven으로 java 프로젝트를 생성해보았습니다. 지금까지는 ```eclipse```IDE로 직접 설치했지만, 메이븐을 사용해보았습니다. 다음에는 maven이 왜 편리한지와 기타 Ant, Gradle이란 빌드툴과 어떤 차이점이 있는 궁금해졌습니다. 감사합니다.
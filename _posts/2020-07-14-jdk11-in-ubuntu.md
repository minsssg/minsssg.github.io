---
layout: post
title: 자바 11버전 설치 in Ubuntu server
description: Ubuntu Server에 자바 11 LTS 버전을 설치해보자!
categories:
    - Linux
comments: true
permalink: jdk11_install_in_ubuntu.html
---
# 자바 11버전 설치 in Ubuntu Server

이번 시간에는 VMware Ubuntu Server에 Java 11버전을 설치해보려고 합니다. 그래서 우선 구글에 검색을 했는데, 대부분 jdk8버전이있고, 다른 버전의 java를 설치하려 했습니다. Oracle 홈페이지에서 찾아보니 java 11 LTS버전이 있어 11버전을 설치하기로 했습니다.

## 개요

Ubuntu Server java 11버전 설치하기 위해 우선 jdk 11.0.7버전을 다운 받고, 우분투 서버에 설치해보겠습니다. 이 자료는 다음 [사이트](https://www.infoworld.com/article/3514725/installing-oracle-java-se-11-on-ubuntu-18-04.html)를 참고했습니다.

## 1. Java 11버전 다운로드

[오라클 사이트](https://www.oracle.com/java/technologies/javase-downloads.html)에 들어갑니다. **Java SE 11(LTS)**를 최신버전에서 JDK Download를 클릭합니다.

<img src="/assets/images/java_install/java_install_00.PNG" alt="java_install_00" style="zoom:60%;" />

그 다음에 여러 운영체제에서 다운받을 수 있는 페이지가 나옵니디ㅏ. 거기서 **jdk-11.0.7_linux-x64_bin.tar.gz**를 다운 받습니다.

<img src="/assets/images/java_install/java_install_01.PNG" alt="java_install_01" style="zoom:60%;" />

다운받은 파일을  ubuntu server downloads 디렉토리로 옮겨 줍니다.

<img src="/assets/images/java_install/java_file_in_download.PNG" alt="java_file_in_download" style="zoom:50%;" />

## 2. Ubuntu server에 자바 설치하기

1. 우선 apt를 업데이터와 업그레이드 해줍니다.

   ```bash
   sudo apt update
   sudo apt upgrade
   ```

   <img src="/assets/images/java_install/apt_update.PNG" style="zoom:50%;" /> <img src="/assets/images/java_install/apt_upgrade.PNG" style="zoom:50%;" />

2. 그런 다음 Downloads 디렉토리로 이동해서 Oracle JDK 11을 ```var```밑에 폴더를 생성해서 옮겨줍니다.

   ```bash
   sudo mkdir -p /var/cache/oracle-jdk11-installer-local/ # 디렉토리를 연속적으로 만듦.
   sudo cp jdk-11.0.7_linux-x64_bin.tar.gz /var/cache/oracle-jdk11-installer-local/
   ```

   <img src="/assets/images/java_install/create_directory.PNG" alt="create_directory" style="zoom:67%;" />

   <img src="/assets/images/java_install/jdk_copy_in_cache.PNG" alt="jdk_copy_in_cache" style="zoom:45%;" />

3. 이제 [PPA(Personal Package Archive)](https://itsfoss.com/ppa-guide/)를 설치하고 update를 합니다. PPA는 자신의 로컬 컴퓨터에 있는 데이터를 설치할 수 있도록 도와주는 툴인거 같습니다.

   ```bash
   sudo add-apt-repository ppa:linuxuprising/java
   sudo apt-get update
   ```

   ![ppa_uprising](/assets/images/java_install/ppa_uprising.PNG)

   ![uprising_install](/assets/images/java_install/uprising_install.PNG)

4. 이제 JDK를 설치해 보겠습니다.

   ```bash
   sudo apt install oracle-java11-installer-local
   ```

   ![java_install_03](/assets/images/java_install/java_install_03.PNG)

   위 그림처럼 명령을 실행하면 설치할 것인지 묻는 문장이 나옵니다. 이 때, ```y```를 통해서 Yes라 하고 계속 진행합니다. 그리고 설치가 진행되고 다음과 같은 그림이 나옵니다.

   ![java_install_04](/assets/images/java_install/java_install_04.PNG)

   ![java_install_05](/assets/images/java_install/java_install_05.PNG)

   java설치를 위한 Oracle License에 대해 동의를 묻는 내용입니다.(~~저도 정확히는 무슨 내용인지 안 읽어봐서 모릅니다 ㅜㅜ~~) 그냥 두 그림 모두 <Ok> , <Yes> 를 선택하면 됩니다.

   ![java_install_06](/assets/images/java_install/java_install_06.PNG)

   그리고 드디어 설치가 끝났습니다!!! 짝짝짝

   이제 자바설치가 잘되었는지 확인하기 위해 다음 명령어를 작성해봅니다.

   ```bash
   java --version
   javac --version
   ```

   ![java_install_07](/assets/images/java_install/java_install_07.PNG)

   ![java_install_08](/assets/images/java_install/java_install_08.PNG)

   원래 Ubuntu도 Windows에서 자바 환경 설정을 해줘야 하는데 여기선 따로 환경설정을 하지 않아도 잘 작동합니다. 왜 그런지는 조금 더 알아볼 필요가 있다고 느껴집니다.

## 3. Hello World in  Java

이제 설치가 다 되었으니 간단히 국룰 아니 세계룰인 ```Hello World```를 찍어보겠습니다. 

  ![hello_world_in_java](/assets/images/java_install/hello_world_in_java.PNG)

이제 ```javac```로 컴파일해보겠습니다.

``` bash
javac HelloWorld.java
```

![java_compile](/assets/images/java_install/java_compile.PNG)

javac로 컴파일을 하면 ```.class```파일이 만들어 집니다. ```.class```파일을 실행하면 다음과 같습니다.

```bash
java HelloWorld
```

![hello_world_run_in_java](/assets/images/java_install/hello_world_run_in_java.PNG)

다음과 같이 결과가 나타납니다.

## 마무리

오늘은 Ubuntu Server에 java 11버전을 설치해보았습니다. 오늘도 조금 비흡한점이 있었습니다. 잘못된 부분, 이상한 점, 오타 등 언제든 말씀해주십시오. 글 읽어 주셔서 감사합니다.
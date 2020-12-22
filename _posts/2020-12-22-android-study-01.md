---
title: "Android 공부"
excerpt: "Android 공부를 해보자!"
author_profile: true
categories:
  - Android
tags:
  - Android
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2020-12-18T13:00-18:00
---

## 1. 개요

안드로이드 개발을 공부를 하게 되었다. 안드로이드는 구글에서 개발한 휴대전화 운영체제이다. 자바와 코틀린으로 할 수 있다고 한다. 

안드로이드를 공부하고 내용을 정리해보도록 하자.

## 2. 안드로이드

안드로이드 앱 구성 4대 컴포넌트

* Activity - 사용자 인터페이스가 있는 단일화면 
* Service - 백그라운드에서 실행
* Content Provider - 데이터 입출력
* Broadcast Receiver - 상태표시줄 알림

컴포넌트 활성화(Intent)

Activity, Service, Broadcast receiver 는 ```Intent```라고 하는 비동기식 메시지를 이용해 활성화 할 수 있다. ```Intent```로 컴포넌트를 실행하는 방법은 다음과 같다.

* Activity - ```startActivity(Intent)```
* Service - ```startService(Intent)```
* Broadcast - ```sendBroadcast(Intent)```
* Content Provider 는 직접 ```Intent```를 날리는 것이 아닌, ```ContentResolver```를 사용해 한 계층을 지나고 활성화 된다.

Android 시스템은 각 앱의 컴포넌트에 대한 정보를 얻기 위해 각 앱```AndroidManifest.xml```파일을 확인해야 한다.

Manifest파일은 다음과 같은 정보를 담고 있다.

* 앱이 가지고 있는 모든 컴포넌트에 대한 정보(컴포넌트 선언)
* 앱이 요구하는 모든 사용자 권한(e.g., 인터넷 액세스, 연락처 액세스)
* 앱에서 사용하거나 필요로하는 하드웨어 및 소프트웨어 기능 선언(ㄷe.g., 카메라, 블루투스)
* 앱이 사용하는 API를 기반으로 앱에서 요구하는 최소 API레벨 선언
* 앱 동작을 위해 링크가 필요한 API라이브러리 선언, 단 Anroid framework API는 제외


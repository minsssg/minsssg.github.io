---
title: "google map api로 주소검색하기-과금문제"
excerpt: "python에서 google map api 과금 문제 발생!"
author_profile: true
categories:
  - python
  - api
tags:
  - python
  - api
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2020-09-20T13:00-18:00
---

## 1. 구글 과금

지난 시간 [google map api로 주소검색하기]() 에서 구글 맵스 api 중 ```Geocoding API```를 사용했습니다. 그런데, ```Geocoding API```로 약 **25,000**건 검색을 사용했습니다. 그리고 몇 일 후, 메일 하나가 날라왔습니다...

![구글 Cloud 결제안내](https://i.imgur.com/3hTLguo.png)

그리고 메일을 확인해보니 <span style="color:orange">**150,000원**</span>치의 금액을 사용했다고 나와있었습니다.... 정말 당황했습니다. 갑자기 많이 사용하긴 했지만 생각보다 많은 금액이 나와서 어떻게 해야 되나 싶었습니다. ㅜㅜ 그래서 구글에 검색도 해보니 Geocoding 한 건당 얼마씩하는 것이 었습니다.

![google 과금 할인](https://i.imgur.com/1Jd1IM8.png)

 하는 수 없이 돈을 지불해야겠다고 생각해서 결제 창에 들어가 보니 **0원**으로 되어있더라구요. 그래서 다시 확인해보니 약 <span style="color:#FF6F61">**350,000원**</span> 프로모션 크레딧이 있었습니다!. 그래서 자동으로 차감이 되어 있더라구요 ^^ 정말 다행이라고 생각했고, 다시 한 번 **갓구글**이라 생각했습니다.

다른 분들도 혹시나 이러한 상황이 발생할 수 있습니다. 항상 요금표를 보시고 본인에게 필요한 서비스를 이용하십시오! 꼭!!
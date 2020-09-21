---
title: "google map api로 주소검색하기"
excerpt: "python에서 google map api 통해 주소를 검색해보자!"
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
last_modified_at: 2020-09-10T13:00-18:00
---

## 1. 개요

가끔 데이터 주소 데이터를 사용하다보면 옛날 주소 혹은 포맷이 다른 주소가 종종 있습니다. 도로명 주소처럼 다르기도 하죠. 

어떤 데이터는 ```김포시 마산동``` 같은 경우 ```김포시 양촌면 마산리``` 였습니다. 이렇게 되면 데이터를 정리하는데 어려움을 겪게 됩니다.(~~해보시면 압니다.~~)

하지만, [**구글 맵스**](https://maps.google.com)에서 검색을 해보면 똑같은 위치에 주소에 맵핑을 해줍니다. 그래서 **google maps api** 의 **Geocoding API**를 써보기로 했습니다.

## 2. Google Maps Platfrom

구글에서는 클라우드 플랫폼으로 다양한 서비스를 제공하고 있습니다. Tensorflow, Firebase, Analytics 등등 그 중에 [**구글 Maps 플랫폼**(google Maps Platform)](https://cloud.google.com/maps-platform?hl=ko)에서 지도와 관련된 API를 제공합니다.

![maps_index_html]()

이후 결제 수단을 등록하고 프로젝트를 만들고 사용할 API를 설정해주면 됩니다. 여기 [블로그](https://happist.com/568746/%EA%B5%AC%EA%B8%80%EB%A7%B5-%ED%99%9C%EC%9A%A9%EB%B2%95-%EA%B5%AC%EA%B8%80%EB%A7%B5-api-key-%EB%B0%9C%EA%B8%89%EB%B0%A9%EB%B2%95)에 자세히 나와있습니다.

## 3. API CALL

이제 ```api_key```를 발급 받았다면 테스트를 해봅시다.

웹 브라우저에서 ```url```검색창에 다음과 같이 입력해봅니다.

```
https://maps.googleapis.com/maps/api/geocode/json?address=[주소]&key=[GEOCODING_API_KEY]
```

```[주소]```는 검색하고 싶은 주소, ```[GEOCODING_API_KEY]```는 아까 발급받은 ```api_key```를 넣어줍니다.

키를 잘 못 넣으면 ```REQUEST_DENIED```를 반환합니다.

**api_key** 에러

![api_key_error]()

예제를 비교해 보겠습니다. 예전 주소인 ```김포시 양촌면 마산리```, 현재 주소인 ```김포시 마산동``` 이 있습니다.

예전 주소를 현재 주소로 반환해주는지 확인해보겠습니다.

![김포시 마산동 검색]()

![김포시 양촌면 마산리]()

포맷이 조금 다르긴 하지만 ```김포시 마산동```이란 글자를 얻을 수 있습니다.

## 4. python에서 Maps API CALL

```python
from urllib.parse import quote_plus, unquote
from urllib.request import Request, urlopen
import json

address = quote_plus("검색할 주소")
API_key = "Geocoding_API_key"

url = 'https://maps.googleapis.com/maps/api/geocode/json?address={}&key={}&language=ko&region=KR'.format(address, API_key)
request = Request(url, headers={"X-Mashape-Key": API_key})
response = urlopen(request)

result = response.read().decode()
json_result = json.loads(result)
```

파이썬은 코드에서 API를 불러올려면 다음과 같다. 검색할 주소와 API로 ```url```을 생성합니다. ```Request```객체를 생성하고 ```urlopen```함수를 통해 ```request```의 결과를 받아옵니다. 이를 ```json```형식으로 반환하는 코드입니다.

## 5. 마무리

오늘은 **Google Maps API**를 통해 주소가 바뀌었거나, 띄어쓰기가 안되어있는 것처럼 주소 포맷이 다른 경우, 해결할 수 있는 방법 하나를 소개했습니다. 더 좋은 방법이 있다면 바로 댓글 ㄲㄲ!  읽어주셔서 감사합니다. 오타, 잘 못 된점도 댓글 환영합니다!
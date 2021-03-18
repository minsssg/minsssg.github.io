---
title: "블로그 테마 및 화면 바꾸기"
excerpt: "요즘 공부도 통 안되고 정리도 안되어 있는 것 같고, 검은색도 지겨워서 블로그 색깔을 바꿔보았습니다."
author_profile: true
categories:
  -jekyll
tags:
  -jekyll
toc: true
toc_sticky: true
toc_label: "페이지 목차"
last_modified_at: 2021-03-18T22:00-23:00
---

# 들어가기

이제 날도 풀리고 봄이 오니까. 아무래도 블로그 색상을 바꿔보고 싶었습니다. 계속 검은색 테마를 사용했는데 가끔은 밝은 창을 보면 생기가 돋는 것 같습니다.

그래서 기존의 ```dark```테마에서 ```light```한 테마로 바꾸고 기타 색상도 바꿔보았습니다.

 ```minimal-mistakes```에서 제공하는 기본 skin을 조금 바꿔 커스터마이징을 해봤습니다.

# Skin 바꾸기

우선 ```_config.xml```파일을 열어서 보면 ```minimal-mistakes-skin```을 볼 수 있습니다.

![image-20210318232955359](https://i.imgur.com/SODFbq4.png) 

skin의 종류로는 "air", "aqua", "contrast", "dark", "dirt", "neon", "mint", "plum", "sunrise" 등이 있습니다. 이 파일들은 ```./_sass/minimal-mistakes/skins```에 위치하고 있습니다.

파일 형식은 ```.scss```입니다. 저는 여기서 ```_mint.scss```를 복사해서 저만의 skin을 만들었습니다. ```_mint.scss```안에 내용은 다음과 같습니다.

```scss
/* ==========================================================================
   Mint skin
   ========================================================================== */

/* Colors */
$background-color: #f5f6f6 !default;
$text-color: #40514e !default;
$muted-text-color: #40514e !default;
$primary-color: #11999e !default;
$border-color: mix(#fff, #40514e, 75%) !default;
$footer-background-color: #30e3ca !default;
$link-color: #11999e !default;
$masthead-link-color: $text-color !default;
$masthead-link-color-hover: $text-color !default;
$navicon-link-color-hover: mix(#fff, $text-color, 80%) !default;

.page__footer {
  color: #fff !important; // override
}

.page__footer-follow .social-icons .svg-inline--fa {
  color: inherit;
}
```

위와 같이 변수에 코드를 바꿔 이제 봄이니까 ```cherry-blossom```이란 ```scss```파일을 만들었습니다.

```scss
/* ==========================================================================
   Cherry Blossom skin
   ========================================================================== */

/* Colors */
$background-color: #f6f6f6 !default;
$text-color: #40514e !default;
$muted-text-color: #40514e !default;
$primary-color: #ffb7c5 !default;
$border-color: mix(#fff, #40514e, 75%) !default;
$footer-background-color: #ffb7c5 !default;
$link-color: #79B3DC !default;
$masthead-link-color: $text-color !default;
$masthead-link-color-hover: $text-color !default;
$navicon-link-color-hover: mix(#fff, $text-color, 80%) !default;

.page__footer {
  color: #fff !important; // override
}

.page__footer-follow .social-icons .svg-inline--fa {
  color: inherit;
}
```

# 결과 화면

1. 리스트 비교화면

   ![asis-tobe](https://i.imgur.com/rIJWYb6.png)

2. 내용 컨텐츠 비교화면

   ![asis-tobe-content](https://i.imgur.com/NphGeAI.png)

```cherry-blossom```의 색깔 코드는 구글에서 "cherry blossom color code"를 검색해서 가져왔습니다

* cherry-blossom : #ffb7c5
* h1태그, a태그 : #79B3DC

# 끝으로

요즘 도통 글도 잘 안써지고 시간이 많이 든다는 점에서 계속 글쓰는 것을 기피하고 있었습니다. 기술공부도 요즘 하지 않아서 더 쓸말도 없었습니다. 그래도 이렇게나마 블로그 색상을 바꾸니 처음 다시 시작하는 느낌도 들고 좋은  것 같습니다. 앞으로도 왕왕 글을 쓰고 좋은 글을 적는 사람이 될 수 있도록 노력하겠습니다. 감사합니다.
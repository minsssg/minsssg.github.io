---
layout: post
title: 새로운 노트북에 gitblog 설치하기
description: 새 노트북에 gitblog를 설치해보자!
categories:
    - github_blog
comments: true
permalink: helloworld.html
---
## Github Blog 설치

새로운 노트북을 샀다. 기존 노트북이 성능이 좋지 않아서 바꿨다.

gitblog를 작성하기 위해 이전([나의 첫 github블로그 게시물!](https://minseok-hub.github.io/helloworld.html))에 정리했던 내용을 다시 보게 되었다.

노트북 환경은 다음과 같다.

* windows 10 home

## jekyll bundle 설치 문제

먼저 ruby를 윈도우 환경([ruby for windows](<https://rubyinstaller.org/downloads/>))에 설치하고 ```start command prompt with ruby```를 실행합니다. 그리고 다음 명령어를 실행해준다.

```bash
gem install jekyll
gem install bundler
# 그 다음 github blog 디렉토리로 이동 후
bundle exec jekyll serve
```

> 여기까지 이전 게시물과 같은 내용이다.

위 명령어를 치면 

> ```Could not find multipart-post-2.1.1 in any of the sources``` 
>
> ```Run `bundle install` to install missing gems```

위 같은 문장이 나왔다. 그래서 ```bundle install```명령을 치니까 파일들이 설치되기 시작했고 다시 ```bundle exec jekyll serve``` 명령이 작동이 됐다. 

jekyll 는 알고보니 local 환경에서 블로그를 변경해볼 수 있는 도구였다. 무튼 명령어를 실행하면 

```Server running... press ctrl-c to stop.``` 이라는 문구가 나왔다!!

이걸 보면 이전과 조금 다른 상황인데 왜 그런지는 모르겠다. 일단 넘어가자!!

그리고 github blog에 push 하면 된다.



처음에는 3시간이나 걸렸지만 다시 해보니 금방해서 좋았다.
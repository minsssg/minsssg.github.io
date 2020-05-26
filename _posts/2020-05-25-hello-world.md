---
layout: post
title: 나의 첫 github블로그 게시물!
description: 이거 쓰는데 3시간 걸림 ㅜㅜ
categories:
    - github_blog
comments: true
permalink: helloworld.html
---
처음 깃 블로그 만들어서 해본다. 이런 블로그는 미리미리 해놓으면 참 좋았을텐데 이걸 왜 이제 하는지 ㅜㅜ... 세월이 아깝다리...

처음 해보는 블로그 jekyll ~~(이건 발음을 어떻게 해야하는겨? 제이킬, 지킬?)~~ 설치와 ruby 설치 그런데 bundler 버전도 안맞아서 꽤 고생했다. 아무튼 내가 고생한 걸 여기서 적어보겠다.

먼저 나의 환경은 다음과 같다.

* windows 10 home

github 블로그 만들고 jekyll 테마를 입혔다.

github 블로그와 jekyll 사용하는 것은 많은 자료들이 이미 있어서 생략한다. ~~(귀찮아서 그런거아님)~~

다만 블로그를 만들다가 문제가 되는 부분만 적었다.



## jekyll bundle 설치 문제

먼저 ruby를 윈도우 환경([ruby for windows](<https://rubyinstaller.org/downloads/>))에 설치하고 ```start command prompt with ruby```를 실행합니다. 그리고 다음 명령어를 실행해준다.

```bash
gem install jekyll
gem install bundler
# 그 다음 github blog 디렉토리로 이동 후
bundle exec jekyll serve
```

위 명령어를 치면 ```Could not find public_suffix-2.0.5 in any of the sources``` 문제가 생겨서 찾아보니 ```gem install public_suffix < "2.0.5"```를 해서 모듈을 설치하면 된단다.... ~~(근데 왜 난 안되냐고 ㅠㅠ)~~ 그렇게 한참 뒤져보다 jekyll 버전과 bundle 버전 차이 때문에 문제가 발생한다고 했다.

그래서 찾아낸 방법이 ```bundle update``` 이걸 실행하니까 ```bundle exec jekyll serve``` 명령이 작동이 됐다. 그리고 나머지는 커스텀 마이즈 방법을 찾아보면서 github blog를 완성했다...

jekyll 는 알고보니 local 환경에서 블로그를 변경해볼 수 있는 도구였다. 무튼 명령어를 실행하면 

```Server running... press ctrl-c to stop.``` 이라는 문구가 나온다. 그리고 local 포트 4000번으로 지정된다. url에 127.0.0.1:4000 검색해보면 테마가 입혀진 블로그가 나오는 걸 볼 수 있을 것이다.

그리고 변경사항이 잘되었다면 자기 github blog에 push 하면 된다.



처음 글이라 두서도 없고 형식도 이상하지만 점점 더 나아지는 나의 모습을 기대하자.e
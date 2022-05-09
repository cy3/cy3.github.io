---
layout: post
title: M1 mac에서 jekyll 설치하기
date: 2021-04-03 08:43:56 +0000
tags: cs jekyll 
---

## ruby 설정
사용하려는 theme에 따라서 ruby 버전이 다르다. 내가 설치하려는 theme은 오래된 것이라 2.5.8 버전을 사용하는데, M1에서는 rbenv 로 ruby 설치가 안 되었다.

삽질을 좀 했는데 결론적으로 다음 명령어를 이용하면 x86이든 apple sillicion이든 설치가 된다.

```bash
RUBY_CFLAGS="-Wno-error=implicit-function-declaration" rbenv install 2.5.8
```

다음 issue를 참고했다.
[Installation issues with Arm Mac (M1 Chip) · Issue #1691 · rbenv/ruby-build · GitHub](https://github.com/rbenv/ruby-build/issues/1691)

나머지 설치는 아래 링크를 참고했다.
[M1 MAC에서 Jekyll 블로깅을 위한 로컬 환경 구축하기 - Yoon Sung's Blog](https://unluckyjung.github.io/develop-setting/2021/01/20/Mac-Jekyll-Setting/)


## local에서 실행
```bash
bundle exec jekyll serve
```


## Theme 추천
[GitHub - aweekj/kiko-now: Build a Jekyll blog in minutes, without touching the command line.](https://github.com/AWEEKJ/kiko-now)

[GitHub - Simpleyyt/jekyll-theme-next: Elegant theme for Jekyll.](https://github.com/simpleyyt/jekyll-theme-next)

## reference
[M1 MAC에서 Jekyll 블로깅을 위한 로컬 환경 구축하기 - Yoon Sung's Blog](https://unluckyjung.github.io/develop-setting/2021/01/20/Mac-Jekyll-Setting/)

[jekyll을 이용한 Github 블로그 만들기](http://labs.brandi.co.kr/2018/05/14/chunbs.html)
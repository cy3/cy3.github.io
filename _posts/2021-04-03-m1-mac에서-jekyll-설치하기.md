---
layout: post
title: M1 mac에서 jekyll 설치하기
date: 2021-04-03 08:43:56 +0000
tags: cs cs/jekyll 
uuid: 27B1E6B4-04B1-4F1B-B833-5EFEA64ECCB9-405-00000C7BC670EDCA
---

## ruby 설정
rbenv 로 ruby를 설치하려고 삽질을 좀 했는데 결론적으로 다음 명령어를 이용하면 x86이든 apple sillicion이든 설치가 된다.

[Installation issues with Arm Mac (M1 Chip) · Issue #1691 · rbenv/ruby-build · GitHub](https://github.com/rbenv/ruby-build/issues/1691)
```bash
RUBY_CFLAGS="-Wno-error=implicit-function-declaration" rbenv install 2.5.8
```


## Theme
[GitHub - aweekj/kiko-now: Build a Jekyll blog in minutes, without touching the command line.](https://github.com/AWEEKJ/kiko-now)

[GitHub - Simpleyyt/jekyll-theme-next: Elegant theme for Jekyll.](https://github.com/simpleyyt/jekyll-theme-next)

## reference
[M1 MAC에서 Jekyll 블로깅을 위한 로컬 환경 구축하기 - Yoon Sung's Blog](https://unluckyjung.github.io/develop-setting/2021/01/20/Mac-Jekyll-Setting/)

[jekyll을 이용한 Github 블로그 만들기](http://labs.brandi.co.kr/2018/05/14/chunbs.html)
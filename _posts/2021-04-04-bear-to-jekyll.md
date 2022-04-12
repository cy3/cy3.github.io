---
layout: post
title: bear to jekyll
date: 2021-04-04 01:24:59 +0000
tags: cs jekyll updating 
---

## bear에서 쓴 글 불러오기
아래 repo를 사용했다.

[GitHub - redspider/pybear: Trivial python library to access Bear Writer notes](https://github.com/redspider/pybear)

그런데 mac os 버전이 올라가서 DB 위치가 바뀌어서 작동이 안 되었다.
해당 repo request에 관련 내용이 있어 수정하였다.

[Fixed path to the sqlite database by harshdeep · Pull Request #11 · redspider/pybear · GitHub](https://github.com/redspider/pybear/pull/11/commits/181501a57fb5e1b2098e0b1bd7d5356d40520336)

header 출력이나 image 출력 문제 등으로 바로 사용이 불가능하여, 조금 수정한다.

## 사용법
bear에서 jekyll로 올리고 싶은 글 들에 `#posts` 태그를 붙인다.
본문 첫 두 줄은 자르기 때문에 (제목과 태그를 본문에서 제거) 제목 다음줄에 태그를 넣어야 한다.
jekyll 폴더에서 다음 스크립트를 실행한다.
```bash
./bear_import.sh
```

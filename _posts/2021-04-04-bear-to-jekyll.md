---
layout: post
title: bear to jekyll
date: 2021-04-04 01:24:59 +0000
tags: cs cs/jekyll 
uuid: DDA24F8D-C016-444A-BA43-3F06DB9C8BA3-405-00000D372C71C935
---

## bear에서 쓴 글 불러오기
아래 repo를 사용했다.

[GitHub - redspider/pybear: Trivial python library to access Bear Writer notes](https://github.com/redspider/pybear)

그런데 mac os 버전이 올라가서 DB 위치가 바뀌어서 작동이 안 되었다.
해당 repo request에 관련 내용이 있어 수정하였다.

[Fixed path to the sqlite database by harshdeep · Pull Request #11 · redspider/pybear · GitHub](https://github.com/redspider/pybear/pull/11/commits/181501a57fb5e1b2098e0b1bd7d5356d40520336)

header 출력이나 image 출력 등에 바로 사용이 불가능하여, 조금 수정하여 pybear 폴더안에 넣었다.

## 사용법
bear에서 jekyll로 올리고 싶은 글 들에 `#posts` 태그를 붙인다.
jekyll 폴더에서 다음 스크립트를 실행한다.

```bash
./bear_import.sh
```

## 결과

![image](/images/6405D5F2-4448-4B94-9434-4477575A2CB7-405-00000E38CE18668D/B934C2A9-82BE-4414-81D4-85820A8CF140.png){: .center-image}
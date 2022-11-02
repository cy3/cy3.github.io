---
layout: post
title: bear를 Jekyll 에디터로 사용하기
date: 2021-04-04 01:24:59 +0000
tags: cs jekyll 
---

## 사용법
### bear
bear에서 jekyll로 올리고 싶은 글 들에 `#posts` 태그를 붙인다.  
제목과 태그를 본문에서 제거하기 위해 본문 첫 두 줄은 자르기 때문에 다음과 같이 제목 다음줄에 태그를 넣어야 한다.
![image](/_images/2021-04-04-bear를-jekyll-에디터로-사용하기/9CE5892B-1D8E-4130-B1DA-CFA9A4C3A38D.png){: .center-image}

### jekyll
jekyll root folder에서 git을 clone하고 스크립트를 링크한다.
```bash
git clone https://github.com/cy3/pybear
ln -s pybear/bear_import.sh .
```
  
해당 프로그램은 `_images` 폴더에 이미지를 저장하게 만들었기 때문에,  `_config.yml` 에 include 설정을 추가해 주어야 한다. 
```yml
include:
  - _images
```

### run
이제 bear에서 글을 쓰고 다음 명령어를 치면 자동으로 포스트가 업데이트된다.
```bash
./bear_import.sh
```



## 구현내용
* [redspider/pybear repo](https://github.com/redspider/pybear)를 사용했다.  
* 그런데 mac os 버전이 올라가서 DB 위치가 바뀌어서 작동이 안 되어서, [repo pull request](https://github.com/redspider/pybear/pull/11/commits/181501a57fb5e1b2098e0b1bd7d5356d40520336)에 관련 내용이 있어 수정하였다. 
* header 출력이나 image 출력등을 커스텀 해서 [cy3/pybear](https://github.com/cy3/pybear) 에 올려두었다.  
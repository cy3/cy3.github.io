---
layout: post
title: git에서 pull 하기 전 파일 잠시 stash 후 불러오기
date: 2022-04-19 09:08:21 +0000
tags: cs git 
---

```bash
# stash 생성
git stash push -m "msg" <file>
# 리스트 확인
git stash list
# pull
git pull
# 복구 및 stash 삭제
git stash pop #stash@{0} 처럼 이름 지정 안할 시 자동으로 마지막 사용
# 충돌 시 confilct 수정 및 아래 내용 실행
$ git reset  # 충돌 해결로 표시 후 색인에서 파일 제거
$ git stash drop
```

## references
[How To Git Stash Changes – devconnected](https://devconnected.com/how-to-git-stash-changes/)
[커밋하지 않고 git stash 충돌을 해결하는 방법](https://rateye.tistory.com/525)
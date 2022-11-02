---
layout: post
title: ipynb useful snippets
date: 2022-10-31 05:10:19 +0000
tags: cs python jupyter 
---

## autoreload 설정
```
%load_ext autoreload
%autoreload 2
```

## 노트북이 위치한 git의 root 폴더로 점프
`gitpython` 설치 필요.

```python
# jump to git root directory
import git, os, sys
root_path = str(git.Repo(os.getcwd(), search_parent_directories=True).working_dir) # type: ignore
print('root_path:', root_path)
os.chdir(root_path)
sys.path.append(root_path)
```

## numpy 정확도 설정
```python
np.set_printoptions(precision=5)
```
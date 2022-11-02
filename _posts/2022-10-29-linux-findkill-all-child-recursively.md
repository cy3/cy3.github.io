---
layout: post
title: linux find/kill all child recursively
date: 2022-10-29 11:08:10 +0000
tags: cs linux 
---

```bash
pid=<target pid>
# pid tree 확인
pstree -p $pid 

# pid만 뽑아서 정렬
pstree -p $pid | perl -ne 'print "$1\n" while /\((\d+)\)/g' | sort -n
# process 목록만 확인
ps -p `pstree -p $pid | perl -ne 'print "$1\n" while /\((\d+)\)/g'`
# thread 목록까지 확인
ps -T -p `pstree -p $pid | perl -ne 'print "$1\n" while /\((\d+)\)/g'`

# 15로 kill
kill -15 `pstree -p $pid | perl -ne 'print "$1\n" while /\((\d+)\)/g'`
# 9로 kill
kill -9 `pstree -p $pid | perl -ne 'print "$1\n" while /\((\d+)\)/g'`
```

## another solution #1
[process - Kill all descendant processes - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/124127/kill-all-descendant-processes)
```bash
function list_descendants() {
  local cpids=`ps -o pid= --ppid "$1"`
  for cpid in $cpids; do list_descendants "$cpid"; done
  echo "$cpids"
}
```
```bash
kill $(list_descendants <target_pid>)
```

## another solution #2
[killing parent process doesn't kill child - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/440691/killing-parent-process-doesnt-kill-child)
```bash
function list_descendants() {
    cpids=`pgrep -P $1|xargs`
    for cpid in $cpids; do list_descendants $cpid; done
    echo "$1"
    #kill -9 $1
}
```
```bash
kill -15 $(list_descendants <target_pid>)
```
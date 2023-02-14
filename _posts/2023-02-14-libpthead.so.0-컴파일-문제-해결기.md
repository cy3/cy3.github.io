---
layout: post
title: libpthead.so.0 컴파일 문제 해결기
date: 2023-02-14 04:05:09 +0000
tags: cs linux 
---

```
Error: compiler_compat/ld: cannot find /lib64/libpthread.so.0: No such file or directory
```

docker로 올린 ubuntu 내에서 컴파일이 필요한 pip 모듈들이 있었는데, libpthread 문제가 자꾸 생겼다.


> *TL;DR* : 맨 아래 conda를 이용한 해결을 참조하자.


기본적으로 우분투에서는 `libpthread-stubs0-dev` 를 깔면 된다.
```bash
sudo apt-get update && sudo apt-get upgrade
sudo apt-get reinstall libpthread-stubs0-dev
```

원래는 해당 패키지를 깔면 `/usr/libx86_64-linux-gnu/` 아래에 라이브러리들이 있어 `/lib64/` 나, `/usr/lib64` 에 링크시켜주면 된다.

...하지만 이번엔 `libpthread_nonshared.a` 파일이 없었다. 여러번 재설치하고 다른 서버에서 받아봐도 `libpthread_nonshared.a` 파일은 찾지 못했다.

`libc6-dev` 에 있다고 하는 글이 있어서, 해당 패키지를 깔고 내용을 확인했는데 여전히 해당 파일은 없었다.

```bash
sudo apt reinstall libc6-dev
dpkg -L libc6-dev
```


## `LD_LIBRARY_PATH` 삽질
열심히 `LD_LIBRARY_PATH` 를 conda sysroot 아래 lib로 바꾸고, pip 코드에서 환경변수를 바꾸기 위해 pip 코드에 들어가 수정까지 했지만 실패했다.

[python - Conda does not look for libpthread and libpthread_nonshared at the right place when compiling inside an env - Stack Overflow](https://stackoverflow.com/questions/71340058/conda-does-not-look-for-libpthread-and-libpthread-nonshared-at-the-right-place-w)

### condapip.patch
```python
index daeb7fb..a1c92b4 100644
--- a/pip/_internal/build_env.py
+++ b/pip/_internal/build_env_modified.py
@@ -128,7 +128,8 @@ class BuildEnvironment:
     def __enter__(self) -> None:
         self._save_env = {
             name: os.environ.get(name, None)
-            for name in ("PATH", "PYTHONNOUSERSITE", "PYTHONPATH")
+            for name in ("PATH", "PYTHONNOUSERSITE", "PYTHONPATH",
+                         "_PYTHON_SYSCONFIGDATA_NAME")
         }
 
         path = self._bin_dirs[:]
@@ -145,6 +146,8 @@ class BuildEnvironment:
                 "PYTHONPATH": os.pathsep.join(pythonpath),
             }
         )
+        if '_PYTHON_SYSCONFIGDATA_NAME' not in os.environ:
+            os.environ['_PYTHON_SYSCONFIGDATA_NAME'] = os.environ['_CONDA_PYTHON_SYSCONFIGDATA_NAME']
 
     def __exit__(
         self,
```

### 패치
```bash
pushd $HOME/PATH_TO_CONDA/conda/envs/ENV_NAME/lib/python3.7/site-packages/ \
    && patch -p0 < $HOME/condapip.patch \
    && popd
```

### version `GLIBC_2.14` not found
라이브러리를 링크하다 보니 glibc가 없다는 에러가 떠서, 직접 빌드할려고 삽질을 했다.
```bash
cd ~/.local
mkdir glibc; cd glibc
wget http://ftp.gnu.org/gnu/glibc/glibc-2.14.tar.gz 
tar zxvf glibc-2.14.tar.gz
cd glibc-2.14
mkdir build; cd build;
../configure --prefix=$HOME/.local/glibc-2.14
make -j4
make install
export LD_LIBRARY_PATH=$HOME/.local/glibc-2.14/lib:$LD_LIBRARY_PATH
```
edit the configure file, look for `3.79* | 3.[89]*`, change it to `3.79* | 3.[89]* | 4.*`
여기서 make 버전이 너무 높아서 안 되서, configure 파일을 열어서 위와 같이 수정했다.
그런데 그걸 고치니 이번엔 gcc 버전이 너무 높아서 안 됬다.

### 참고: path에서 library 있는지 확인하는 코드
```bash
IFS=':' ; for i in $LD_LIBRARY_PATH; do ls -l $i/libpthread.so.0 2>/dev/null;done
```


## conda를 이용한 해결
conda에서 compiler를 설치하면 `conda/envs/ENV_NAME/x86_64-conda-linux-gnu/sysroot/usr/lib64/`  폴더에 `libpthread.so.0` , `libpthread_nonshared.a` 파일 등이 모두 있다.
해당 파일들을 각각 `/lib64/` 와, `/usr/lib64` 에 링크시켜주면 *마참내* pip로 설치가 되는 것을 확인할수 있었다.

```bash
export $CONDA_SYSLIB=$HOME/PATH_TO_CONDAENV/x86_64-conda-linux-gnu/sysroot/usr/lib64/
sudo ln -s $CONDA_SYSLIB/libpthread.so.0 /lib64/libpthread.so.0
sudo ln -s $CONDA_SYSLIB/libpthread_nonshared.a /usr/lib64/libpthread_nonshared.a
sudo ln -s $CONDA_SYSLIB/libc.so.6 /lib64/libc.so.6
sudo ln -s $CONDA_SYSLIB/libc_nonshared.a /usr/lib64/libc_nonshared.a
```
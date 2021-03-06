---
title:      pyenv 加速下载 Python
date:       2020-04-26
tags:
    - Mac
    - Python
    - pyenv
---
## 一、安装 pyenv
```bash
# 安装 pyenv
curl https://pyenv.run | bash
# 重启 shell
exec $SHELL
```

## 二、创建 pyenv_install.sh 文件
```bash
touch pyenv_install.sh
# 添加执行权限
chmod u+x pyenv_install.sh
```

>在 pyenv_install.sh 添加以下内容

```bash
#!/bin/bash
version=${1}

if [ ! $version ]
then
    exit 0
fi

build_cache_path="$HOME/.pyenv/cache"

if [ ! -x $build_cache_path ]
then
    mkdir $build_cache_path
fi

file_name="Python-$version.tar.xz"
file_path=$build_cache_path/$file_name

if [ ! -f $file_path ]
then
    url="https://npm.taobao.org/mirrors/python/$version/$file_name"
    wget $url -P $build_cache_path
fi

pyenv install $version
```

## 三、使用 pyenv_install.sh 安装 python
```bash
pyenv_install.sh 3.7.4
```
![](https://raw.githubusercontent.com/terrluo/terrluo.github.io/master/img/2020-04-26-安装python.png)

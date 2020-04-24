---
title:  "Mac py2 安装 mysql-python"
date:   2020-04-24 21:03:36 +0800
categories: Mac Django
---
1.安装 mysql@5.7(不知道是不是必须，我只用 5.7 版本成功安装 mysql-python 过)

```bash
# 如果已经安装了MySQL 8，先进行卸载
brew uninstall mysql
brew uninstall mysql@5.7
rm -rf /usr/local/var/mysql
rm /usr/local/etc/my.cnf

# 安装MySQL 5.7
brew install mysql@5.7
# 强行link
brew link --force mysql@5.7
# 启动服务
echo 'export PATH="/usr/local/opt/mysql@5.7/bin:$PATH"' >> ~/.zshrc
mysql.server start
# 重置密码
mysql_secure_installation
````

2.设置 openssl

```bash
# mac 应该是自带了 openssl
export LDFLAGS="-L/usr/local/opt/openssl/lib"
export CPPFLAGS="-I/usr/local/opt/openssl/include"
```

3.安装 mysql-python

```bash
pip install mysql-python==1.2.5
```

---
title: 'configure: error: Cannot find OpenSSL''s <evp.h>'
date: 2017-03-29 16:49:55
---

进行编译安装PHP-mongodb扩展, 执行 `./configure` 时提示如下错误信息:

```shell
configure: error: Cannot find OpenSSL's <evp.h>
```
因为执行的PHP是MAMP里集成的, 缺少OpenSSL，只有编译安装的PHP才有OpenSSL，偷个懒直接使用brew下载一个PHP。执行 `configure` 时添加如下参数(换成homebrew下载的php-openssl目录)如下命令：

```Shell
 ./configure --with-openssl-dir=/usr/local/Cellar/openssl/1.0.2k
```
---
title: "PHP 扩展安装失败"
author: "Paco"
tags: ["PHP"]
date: 2018-08-26T19:19:42+08:00
draft: false
---


### 1. Connection to ssl://pecl.php.net:443 fail

> 安装 pecl install redis,  pecl install mongodb, pecl search redis, pecl install swoole, 提示以下错误

`No releases available for package "pecl.php.net/memcache"`

导致出现这个错误的原因，可能是网络问题（需要翻墙）或者是 `OpenSSL` 的问题，这里提供一个简单的办法：

- 从 [pecl.php.net](https://pecl.php.net/) 搜索下载所需要的扩展包（比如下载一个 Redis 的扩展， 会得到 **redis-4.1.1.tgz** 这个压缩包）
- 执行 `pecl install redis-4.1.1.tgz`



### 2. configure: error: Cannot find OpenSSL's \<evp.h\>

> 错误信息

```shell
configure: error: Cannot find OpenSSL's <evp.h>
ERROR: `/private/tmp/pear/temp/event/configure --with-php-config=/usr/local/opt/php@7.0/bin/php-config --enable-event-debug=no --enable-event-sockets=yes --with-event-libevent-dir=/usr --with-event-pthreads=no --with-event-extra --with-event-openssl --with-event-ns=no --with-openssl-dir=no' failed
```

> 解决办法

```shell
$ brew install openssl

$ ln -s /usr/local/Cellar/openssl/1.0.2m/include/openssl /usr/local/include/openssl

$ export LDFLAGS="-L/usr/local/opt/openssl/lib"

$ export CFLAGS="-I/usr/local/opt/openssl/include"
```



### 3. configure: error: Please reinstall the mosquitto distribution

在安装 `mosquitto` 扩展的时候总是 error

> 错误信息如下

```shell

configure: error: Please reinstall the mosquitto distribution
ERROR: `/private/tmp/pear/temp/Mosquitto/configure --with-php-config=/usr/local/opt/php@7.0/bin/php-config --with-mosquitto' failed

```

>  解决办法: 原来是这个扩展包会依赖本地的 Mosquitto Broke 才能进行安装，MacOS 可用 `Homebrew` 直接安装
>
>  错误提示信息，类似这种 `reinstall` `distribution` 都可以考虑按照以下方法尝试解决

```shell
brew install mosquitto

pecl install mosquitto
```


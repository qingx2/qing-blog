---
title: "Linux部署PHP项目环境"
author: "Paco"
tags: ["PHP", "Linux"]
date: 2018-11-06T15:55:32+08:00
draft: false
---

- 全新 Linux 服务器部署记录
- 使用 Yum、Systemd 进行各依赖的管理
- 主要记录：安装步骤、依赖配置、可能遇到的问题
- 涉及软件：PHP、MySQL、Nginx、MongoDB、Redis、Mosquitto、Git

<!--more-->

### 更新yum

```javascript
yum update
```



### 安装 Nginx

- 下载安装

```javascript
 yum -y install nginx
```

- 设置开机自启、启动服务

```javascript
systemctl enable nginx
systemctl start nginx
```



### 安装 PHP PHP-fpm

- 首先需要安装 PHP 源

```javascript
 rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm 
 rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
```

- 下载安装

```javascript
 yum -y install php70w php70w-fpm
```

- 设置开机自启、启动服务

```javascript
systemctl enable php-fpm
systemctl start php-fpm
```

- 安装常用 PHP 扩展

```javascript
yum -y install php70w-mysql php70w-pecl-redis php70w-pecl-mongodb php70w-mbstring php70w-bcmath
```



### 安装 MongoDB

- 指定 MongoDB 版本 (无特殊要求可省略)

> 因为 yum 默认安装的是版本是 2.6.12，我需要 3.* 的版本，需要配置一下 repo

```shell
 vi /etc/yum.repos.d/mongodb-org-3.4.repo

 # 将以下配置复制进

 [mongodb-org-3.4]
 name=MongoDB Repository
 baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
 gpgcheck=0
 enabled=1
```

- 下载安装

```javascript
yum install -y mongodb-org
```

- 查看版本是否正常安装

```shell
mongo --version

MongoDB javascript version v3.4.17
git version: 7c14a47868643bb691a507a92fe25541f998eca4
OpenSSL version: OpenSSL 1.0.1e-fips 11 Feb 2013
allocator: tcmalloc
modules: none
build environment:
    distmod: rhel70
    distarch: x86_64
    target_arch: x86_64
```

- 使用 javascript 交互验证是否正常启用

```shell
# 进入 javascript 环境
mongo

# 查看帮助信息
help

# 查看当前所有数据库
show dbs
```

- 设置开机自启、启动服务

```javascript
systemctl start mongod
systemctl status mongod
```

- 密码权限设置，创建 root 用户

```shell
$ mongo
> use admin
> db.createUser({user:'root',pwd:'root',roles:['root']})

# 分配其它数据库的权限 (可选)
> db.grantRolesToUser("root",[{ role: "readWrite", db: "iStrip" },{ role: "dbAdmin", db: "iStrip" }])

# 指定用户指定数据库权限 (可选)
db.createUser({user:'power',pwd:'highPower4Mongo',roles:['readWrite','dbAdmin']})
```

- 开启权限验证

```shell
$ vi /etc/mongod.conf

# 添加如下配置

security:
  authorization: "enabled"
```

- 重启生效

```javascript
systemctl restart mongod.service
```



### 安装 MariaDB

- 下载安装

```javascript
yum install mariadb mariadb-server
```

- 设置用户权限

- 设置开机自启、启动服务

```javascript
systemctl start mariadb
systemctl status mariadb
```



### 安装 Redis

- 下载安装

```javascript
yum install redis
```

- 配置 `/etc/redis.conf`

```shell
# Auth 权限密码
requirepass highPower4Redis

# 开启 AOF 模式，数据持久化
appendonly yes
```

- 设置开机自启、启动服务

```javascript
systemctl start redis
systemctl status redis
```



### 安装 Mosquitto

- 下载安装

```javascript
yum install -y libmosquitto-devel
```

- 创建用户、密码

```javascript
mosquitto_passwd -c /etc/mosquitto/pwfile userName
```

- 开启权限验证

```shell
vim /etc/mosquitto/mosquitto.conf

# 修改以下参数
allow_anonymous false
password_file /etc/mosquitto/pwfile
```

4. 设置开机自启、启动服务

```javascript
systemctl start mosquitto
systemctl status mosquitto
```

5. 安装 PHP mosquito扩展 (遇到的意外错误)  `pecl install Mosquitto-0.4.0`



```shell
WARNING: channel "pecl.php.net" has updated its protocols, use "pecl channel-update pecl.php.net" to update
downloading Mosquitto-0.4.0.tgz ...
Starting to download Mosquitto-0.4.0.tgz (23,804 bytes)
........done: 23,804 bytes
5 source files, building
running: phpize
Can't find PHP headers in /usr/include/php
The php-devel package is required for use of this command.
ERROR: `phpize' failed

解决办法:

$ yum install php70w-devel
```



```javascript
configure: error: in `/var/tmp/pear-build-rootOGtRth/Mosquitto-0.4.0':
configure: error: no acceptable C compiler found in $PATH
See `config.log' for more details
ERROR: `/var/tmp/Mosquitto/configure --with-php-config=/usr/bin/php-config --with-mosquitto' failed


解决办法: 

$ yum install gcc
```



```javascript
configure: error: Please reinstall the mosquitto distribution
ERROR: `/var/tmp/Mosquitto/configure --with-php-config=/usr/bin/php-config --with-mosquitto' failed


解决办法: 

# 添加 MQTT yum源
$ wget http://download.opensuse.org/repositories/home:/oojah:/mqtt/CentOS_CentOS-7/home:oojah:mqtt.repo
# 服务器上的软件包信息缓存起来
$ yum makecache
# 安装
$ yum install libmosquitto-devel
```



### 安装 Git

```javascript
yum install git
```
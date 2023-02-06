---
title: "Redis 配置使用守护进程启动 sentinel"
author: "Paco"
tags: ["Redis"]
date: 2017-06-29T14:01:50+08:00
draft: false
---

#### 概述

​	Redis-Sentinel是Redis官方推荐的高可用性(HA)解决方案，当用Redis做Master-slave的高可用方案时，假如master宕机了，Redis本身(包括它的很多客户端)都没有实现自动进行主备切换，而Redis-sentinel本身也是一个独立运行的进程，它能监控多个master-slave集群，发现master宕机后能进行自懂切换。

<!--more-->

- 不时地监控redis是否按照预期良好地运行;
- 如果发现某个redis节点运行出现状况，能够通知另外一个进程(例如它的客户端);
- 能够进行自动切换。当一个master节点不可用时，能够选举出master的多个slave(如果有超过一个slave的话)中的一个来作为新的master,其它的slave节点会将它所追随的master的地址改为被提升为master的slave的新地址。

#### Sentinel状态持久化

snetinel的状态会被持久化地写入sentinel的配置文件中。每次当收到一个新的配置时，或者新创建一个配置时，配置会被持久化到硬盘中，并带上配置的版本戳。这意味着，可以安全的停止和重启sentinel进程。



#### 配置

此 .conf 配置文件为HomeBrew默认文件路径

> vi /usr/local/etc/redis-sentinel.conf

```shell
// 添加如下配置
daemonize yes //开启守护进程
logfile "/var/log/sentinel_log.log" //配置日志记录
```



#### 运行Sentinel

sentinel默认监听26379端口，所以运行前必须确定该端口没有被别的进程占用。

```shell
redis-server /path/to/sentinel.conf --sentinel
```


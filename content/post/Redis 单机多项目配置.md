---
title: "Redis 单机多项目配置"
author: "Paco"
tags: ["Redis"]
date: 2018-08-22T14:01:50+08:00
draft: false
---


```shell
vim /etc/redis_6380.conf

cp /etc/redis.conf /etc/redis_6380.conf
```

修改以下配置:

```
prot=6380
logfile /var/log/redis/redis_6380.log
pidfile /var/run/redis_6380.pid
dbfilename dump_6380.rdb
appendonly yes
appendfilename "appendonly_6380.aof"
```

启动 Redis 服务

```shell
redis-server /etc/redis_6380.conf
```


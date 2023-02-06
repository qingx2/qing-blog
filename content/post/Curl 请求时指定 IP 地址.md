---
title: "Curl 请求时指定 IP 地址"
author: "Qing"
# cover: "/img/cover.jpg"
tags: ["Curl"]
date: 2021-10-18T15:05:52+08:00
draft: true
---

有时需要清理线上 Opcache 缓存, 或遇到需要调试指定的服务器场景, 就可以使用 Curl v7.21.3 自带的命令 `--resolve` 来辅助完成。

下面的指令意思是将 `example.com` 的请求强制指向到 `127.0.0.1`  IP 地址上进行处理

`curl http://domain.com --resolve domain.com:80:127.0.0.1`


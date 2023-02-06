---
title: Mysql常用命令
date: 2017-11-22 17:06:39
---



##### 表结构-数据导出

mysqldump -uroot -piMariadb168 --databases aquarium --tables aq_region aq_chip  >/tmp/db1.sql;
---
title: "TXT文件导入到MySQL"
date: 2020-07-10T17:18:59+08:00
draft: false
---

#### 在登录 MySQL 客户端时，开启导入本地文件数据的配置

这个步骤不开启的话，可能出现此错误 `Query 1 ERROR: The MySQL server is running with the --secure-file-priv option so it cannot execute this statement`

```shell
mysql --local-infile -uroot
```

#### 执行导入命令

```mysql
LOAD DATA local INFILE '/path/filename.txt' INTO TABLE `table_name`;
```


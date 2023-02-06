---
title: "MySQL重置密码"
tags: ["MySQL"]
date: 2018-08-04T17:05:54+08:00
draft: false
---

### 问题描述

今天在 macOS 上使用 Homebrew 安装 MySql ，正常情况下 MySql 会默认分配默认 root 账户 root 密码，但如果之前安装过MySql修改过账户密码，并且后面删除 MySql 服务，因为配置文件还在，所以这个时候不会产生 root 密码，但当自己在终端上使用默认密码登录的时候，总会提示一个授权失败的错误：

```shell
Access denied for user  'root'@'localhost' (using password)
```

<!--more-->

解决方案
既然现在没法登录到数据库中，改密码和添加用户等操作也无从谈起。好在 MySQL 中还提供了一种免去密码校验进入数据库的方法，我们就先使用这种方法登入到数据库中。然后将默认密码替换掉，上面的问题就可以解决掉啦~ 具体操作如下（如果想要快速解决，可以直接看最下面的快速方案）

一、修改 Mysql 配置文件
mac 系统中配置文件是 mysql 安装目录 support_file 下的 my-default 文件 


需要注意的是：默认该配置文件不具备写权限需要使用 chmod 命令先为该文件添加写权限才能进行更改

ps:windows 系统的配置文件是 mysql 安装根目录的 my.ini 文件

二、修改配置文件
打开刚才我们找到的配置文件，然后在里面找到 [mysqld] 这一项，然后在该配置项下添加 skip-grant-tables 这个配置，然后保存文件。 
这里写图片描述

三、重启 mysql 服务
为了使上一步的配置项生效，我们需要重启 MySQL 的服务 
Mac 系统可以在系统偏好中进行重启： 
这里写图片描述
windows 系统可以通过：在我的电脑上右键–> 服务–> 找到 mysql 服务进行重启 
linux 系统可以使用：service mysqld restart 来重启

四、免密登录 MySQL
然后再次进入到终端当中，敲入 mysql -u root -p 命令然后回车，当需要输入密码时，直接按 enter 键，便可以不用密码登录到数据库当中

五、修改默认的密码
使用 set password for 'username'@'host' = password('newpassword') 命令修改新的密码。

根据网友 Marksmanbat 评论，如果在执行该步骤的时候出现ERROR 1290 (HY000): The MySQL server is running with the --skip-grant-tables option so it cannot execute this statement 错误。则执行下 flush privileges 命令，再执行该命令即可。

六、检验成果
我们改完默认密码后，再次进入到之前的配置文件中，将我们跳过密码的那个配置行给删除掉，变为系统原先的配置。重启 MySQL 服务，下次再登录的时候便可以解决掉这个问题了。

PS 另辟蹊径：该问题快速解决方案
要是你觉得上面的操作过于麻烦，可以使用下面的快捷方式达到上面的效果，针对 mac 系统为例： 
首先进入到 /usr/local/mysql/support-file 这个目录下, 然后按照图片上的步骤进行操作

这里写图片描述
进入 mysql 的安全模式后，键入图中圈起来的四行配置（必须逐行输入），输入完成后使用 contrl+z 键结束输入 
然后再终端中使用 mysql -u root -p 同样可以实现密码登录，另外此时密码也已经修改为了 pass ，下次的登录即可使用 pass 这个新密码了。与上面的操作达成的效果是相同的。****

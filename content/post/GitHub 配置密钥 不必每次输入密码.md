---
title: GitHub 配置秘钥SSH
date: 2017-03-23 22:40:31
---

##### 1、查看电脑是否已经有 SHH key

终端输入:

~~~
➜  cd ~/.ssh
➜  ls
id_rsa      id_rsa.pub  known_hosts
~~~

如果有 **id_rsa.pub** 或者 **id_dsa.pub** 任一文件, 那么就不需要再生成SSH key, 直接进行第3步就行。

<!--more-->

##### 2、创建 SSH key

~~~shell
ssh-keygen -t rsa -C "your_email@mail.com"
~~~

代码参数:

-t 密钥类型, 默认rsa

-C 注释

-f 指定密钥文件存储的文件名,不加此参数运行后仍然会让你输入一个文件名,按下enter键使用默认文件名就行。



会提示你输入两次密码(此密码是push文件时输入的密码,不是GitHub的登录密码),也可以直接enter。接下来会将SSH key提示给你：

~~~
# The key fingerprint is:
# 01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@example.com
~~~

##### 3、登录GitHub在个人中心打开 **SSH and GPG keys** 页面点击 **New SSH key** 按钮将 SSH key 粘贴到页面Key区域内

![](http://omixc2ggv.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-03-23%2022.08.33.png)

##### 4、查看是否已正常使用SSH

终端输入:

~~~shell
➜  .ssh ssh -T git@github.com
~~~

enter回车后,会见到字段,就表示已经成功了:

~~~
Hi SurprisePeas! You've successfully authenticated, but GitHub does not provide shell access.
~~~


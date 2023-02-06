---
title: Mac vi 无法编辑~/.base_profile(配置全局变量)
date: 2017-03-29 17:08:46
---

vi编辑 `.base_profile` 文件时,提示如下信息:

```shell
E325: ATTENTION
Found a swap file by the name ".bash_profile.swp"
       owned by: root   dated: Tue Nov 10 17:42:38 2017
       file name: /etc/proftpd/etc/proftpd.conf
       modified: no
       user name: root   host name: localhost
       process ID: 11195
While opening file ".bash_profile"
             dated: Tue Nov 10 18:32:16 2017
      NEWER than swap file!
(1) Another program may be editing the same file.
    If this is the case, be careful not to end up with two
    different instances of the same file when making changes.
    Quit, or continue with caution.
(2) An edit session for this file crashed.
    If this is the case, use ":recover" or "vim -r .bash_profile"
    to recover the changes (see ":help recovery").
    If you did this already, delete the swap file ".bash_profile.swp"
    to avoid this message.
```

提示说明是因为上次在编辑次文件时意外退出, 并生成了一个`.bash_profile.swp` 文件, 如果已经确定无误, 删除 `.bash_profile.swp`文件就可以正常打开 `.bash_profile`了。
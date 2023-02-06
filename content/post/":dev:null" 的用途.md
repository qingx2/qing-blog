---
title: " /dev/null 的用途"
author: "风 fworld $ 雪狼湖"
tags: ["/dev/null", "Linux"]
date: 2018-10-15T21:49:43+08:00
draft: false
---

`/dev/null`从名称上可以很显然看出是一个空文件（写入到 `/ dev/null` 时全部丢失，读取 `/ dev/null` 时自己返回 `EOF`），那么你会很疑惑，他到底有什么用途呢，请看下文听我讲解，可能你在很多脚本里看过 `/dev/null`，具体总结下几种常见用途.

<!--more-->

### 清除文本文件内容

```shell
cat /dev/null > /var/log/messages

# 有同样的效果, 但不会产生新的进程.（因为: 是内建的）
: > /var/log/messages
```



### 禁止标准输出

```shell
# 文件内容丢失，不会输出到标准输出
cat $filename >/dev/null
```



### 禁止标准错误

```shell
# 删除文件错误时，不会再有提示到终端，都丢到 / dev/null 里去了
rm $badname 2>/dev/null
```



### 禁止标准输出和标准错误的输出

```shell
# 如果 "$filename" 不存在，将不会有任何错误信息提示
# 如果 "$filename" 存在, 文件的内容不会打印到标准输出
# 因此, 上面的代码根本不会输出任何信息
# 当只想测试命令的退出码而不想有任何输出时非常有用
cat $filename 2>/dev/null >/dev/null
```

```shell
#----------- 测试命令的退出 begin ----------------------#  
ls dddd 2>/dev/null 8 
echo $?    // 输出命令退出代码：0 为命令正常执行，1-255 为有出错。  

#----------- 测试命令的退出 end-----------#    
cat $filename &>/dev/null     
```



### 隐藏 cookie 而不再使用

> 所有的 cookies 都会丢弃, 而不会保存在磁盘上了

```shell
if [-f ~/.netscape/cookies]  # 如果存在则删除
then
  rm -f ~/.netscape/cookies  
fi
  ln -s /dev/null ~/.netscape/cookies 
```


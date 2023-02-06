---
title: "PHP Array strpos 实现方式"
author: "Qing"
tags: ["PHP"]
date: 2021-11-18T10:34:45+08:00
draft: true
---

##### 如何在 PHP 中快速检测一个字符串中的值是否存在于指定的多个特征值呢?

*物料*

```php
$needles = ['facebook', 'twitter', 'google'];
$str = 'www.google.com.hk';

if (array_strpos($str, $needles) !== false) {
  echo 'Got the needle';
} else {
  echo 'The needle was not found!';
}
```



`array_strpos` 实现

```php
function array_strpos(string $str,array $needles): bool
{
 	return str_replace($needles, '', $str) !== $str;
}
```

*不足之处就是无法直接获取键*


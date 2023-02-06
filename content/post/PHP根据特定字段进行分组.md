---
title: PHP根据特定字段进行分组
date: 2017-07-28 11:46:28
---

**如果你的数据是这样:**

![](http://omixc2ggv.bkt.clouddn.com/WechatIMG13.jpeg)


**需要转换为如下格式**

![](http://omixc2ggv.bkt.clouddn.com/WechatIMG14.jpeg)


**添加此函数可以进行转为**

```php
public function array_group_by($arr, $key)
{
  $grouped = [];
  foreach ($arr as $value) {
    $grouped[$value[$key]][] = $value;
  }

  // 递归重组嵌套数组 
  if (func_num_args() > 2) {
    $args = func_get_args();
    foreach ($grouped as $key => $value) {
      $parms = array_merge([$value], array_slice($args, 2, func_num_args()));
      $grouped[$key] = call_user_func_array('array_group_by', $parms);
    }
  }

  return $grouped;
}
```


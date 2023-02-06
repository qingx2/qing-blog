---
title: php7新特性学习
date: 2017-07-01 00:36:44
---

*参考[PHP官网](http://php.net/manual/zh/migration70.new-features.php)个人记录学习*

<!--more-->

#### null合并运算符

```php
// 支持链式语法操作,会返回第一个有值的数据
$username = $_GET['user'] ?? $_POST['user'] ?? 'nobody';

```

<br/>

#### 返回值类型声明

```php
// 返回类型声明指明了函数返回值的类型
function arraysSum(array ...$arrays): array
{
    return array_map(function(array $array): int {
        return array_sum($array);
    }, $arrays);
}

print_r(arraysSum([1,2,3], [4,5,6], [7,8,9]));

// 输出结果
Array
(
    [0] => 6
    [1] => 15
    [2] => 24
)
```



#### 太空船操作符（组合比较符）

```php
echo "a" <=> "a"; // 0
echo "a" <=> "b"; // -1
echo "b" <=> "a"; // 1

echo 1 <=> 1; // 0
echo 1 <=> 2; // -1
echo 2 <=> 1; // 1
```



#### 匿名类

通过*new class* 来实例化一个匿名类，传递参数到匿名类的构造器，也可以扩展（extend）其他类、实现接口（implement interface），这可以用来替代一些“用后即焚”的完整类定义。

```php
<?php
interface Logger {
    public function log(string $msg);
}

class Application {
    private $logger;

    public function getLogger(): Logger {
         return $this->logger;
    }

    public function setLogger(Logger $logger) {
         $this->logger = $logger;
    }
}


$app = new Application;
$app->setLogger(new class implements Logger {
    public function log(string $msg) {
        echo $msg;
    }
});

var_dump($app->getLogger());
?>
```



#### 同一 `namespace` 可使用 `use` 一次性引进

```php
<?php

// PHP 7 之前的代码
use some\namespace\ClassA;
use some\namespace\ClassB;
use some\namespace\ClassC as C;

use function some\namespace\fn_a;
use function some\namespace\fn_b;
use function some\namespace\fn_c;

use const some\namespace\ConstA;
use const some\namespace\ConstB;
use const some\namespace\ConstC;

// PHP 7+ 及更高版本的代码
use some\namespace\{ClassA, ClassB, ClassC as C};
use function some\namespace\{fn_a, fn_b, fn_c};
use const some\namespace\{ConstA, ConstB, ConstC};
?>

```



#### 新增整数除法函数 `intdiv()`

```php
<?php

var_dump(intdiv(10, 3));

// 输出
int(3)
?>
```



#### 新增 `preg_replace_callback_array` 

可在次函数体内使用数组形式执行多个 正则表达式搜索并且使用回调进行替换的功能

类似[函数](http://php.net/manual/zh/function.preg-replace-callback.php) `preg_replace_callback`

```php
<?php
$subject = 'Aaaaaa Bbb';

preg_replace_callback_array(
    [
        '~[a]+~i' => function ($match) {
            echo strlen($match[0]), ' matches for "a" found', PHP_EOL;
        },
        '~[b]+~i' => function ($match) {
            echo strlen($match[0]), ' matches for "b" found', PHP_EOL;
        }
    ],
    $subject
);
?>

```



#### 新增2个高安全级别的生成随机数函数

```php
<?php
// 生成适合加密使用的加密随机字节的任意长度字符串，如生成盐、密钥或初始化向量时的字符串。 random_bytes()
$bytes = random_bytes(5);
var_dump(bin2hex($bytes)); // string(10) "385e33f741"
  

// 生成随机整数 random_int()
var_dump(random_int(100, 999)); // int(248)
var_dump(random_int(-1000, 0)); // int(-898)

?>
```












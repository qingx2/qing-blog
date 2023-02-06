---
title: PHP 面试题纪录
date: 2017-05-21 10:32:37
---

#### 数据库

##### 连接数据库

```php
$DB = new PDO("mysql:host=localhost;dbname=mydatabase","root","");
$DB->query("SET NAMES UTF8");
```

<!--more-->

##### MyISAM与InnoDB索引的区别，还有哪些索引方式？

```scss
1. MyISAM引擎是一种非事务性的引擎，提供高速存储和检索，以及全文搜索能力，适合数据仓库等查询频繁的应用
2. InnoDB则是一种支持事务的引擎。给MySQL提供了具有提交，回滚和崩溃恢复能力的事务安全（ACID兼容）存储引擎。
3. hash索引、B-Tree索引、Full-text索引。。。
```

##### 优化查询的过程是怎么样的？

```scss
0. 先运行看看是否真的很慢，注意设置SQL_NO_CACHE
1. where条件单表查，锁定最小返回记录表。这句话的意思是把查询语句的where都应用到表中返回的记录数最小的表开始查起，单表每个字段分别查询，看哪个字段的区分度最高
2. explain查看执行计划，是否与1预期一致（从锁定记录较少的表开始查询）
3. order by limit 形式的sql语句让排序的表优先查
4. 了解业务方使用场景
5. 加索引时参照建索引的几大原则
6. 观察结果，不符合预期继续从0分析
```

##### 两张表 city表和province表。分别为城市与省份的关系表。

```mysql
city:
        id City provinceid

        1  广州 1

        2  深圳 1

        3  惠州 1

        4  长沙 2

        5  武汉 3

```

```mysql
province:
        id province

        1  广东

        2  湖南

        3  湖北
```


（1） 写一条sql语句关系两个表，显示字段：城市id ，城市名， 所属省份 。如：

​           `Id（城市id） Cityname(城市名) privence(所属省份)`

（2）如果要统计每个省份有多少个城市，请用group by 查询出来, 显示字段：省份id ，省份名，包含多少个城市。


```sql
1.select A.id,A.Cityname,B.province from city A,province B where A.provinceid=B.id

2.select B.id,B.province,count(*) as num from city A,province B where A.provinceid=B.id group by B.id

```



##### 取出表A中第10到第20记录（注意：ID可能不是连续的）

表1: id, name, typeid, titre;表2: typeid ,typename
查询 表1的全部字段 和表2 的typename 要求实现分页，每页要有10条数据，查询第二页 写出查询的select语句

```sql
SELECT TOP 10 * FROM (select a.*,b.typename from 表1 a left join 表2 b on a.typeid=b.typeid) a
WHERE ID >(SELECT MAX(id) FROM (SELECT TOP 20 id FROM  表1 ORDER BY id) b) ORDER BY ID
```



#### 服务器

##### Nginx与Apache的区别

``` Scss
Nginx相对于Apache比较来说:
  轻量级，同样是 web 服务，比apache占用更少的内存及资源 
  抗并发，nginx 处理请求是异步非阻塞的，而apache 则是阻塞型的，在高并发下nginx 能保持低资源低消耗高性
能,Nginx是epoll网络IO, Apache是select模型，所以处理请求起来效率会低很多。
  高度模块化的设计，编写模块相对简单,配置文件简洁
  Nginx具有高性能、轻量级、内存消耗少及强大的负载均衡能力
```



#### PHP



##### echo(),print(),print_r()的区别

```php
echo是PHP语句, print和print_r是函数,语句没有返回值,函数可以有返回值(即便没有用)

print（） 只能打印出简单类型变量的值(如int,string)

print_r（） 可以打印出复杂类型变量的值(如数组,对象)

echo 输出一个或者多个字符串
```



##### include和require有什么区别？

```Php
区别在于他们如何处理失败，如果require的文件没有找到，会造成fatal error，脚本停止执行，如果include的文件没有找到，会显示警告，但是脚本会继续往下执行。
```

##### 给一个变量赋值为0123，但是输出该变量的值总是为其他数字

```Php
PHP 解释器会把以0开始的数字当做是八进制的，所以它的值会变成八进制的。
```

##### 如何对一个变量进行值传递？

```Php
可以像C++那样， 在变量的前面加上&, 例如：$a = &$b
```

##### 请描述$message和$message的区别，并输出这两个变量的结果。

```Php
$message是个简单的变量，$$message是变量的变量，表示这个变量的值。
```

##### htmlentities() 和 htmlspecialchars()有什么区别

```Php
htmlspecialchars只转化<、>、 单引号、双引号、&符号

htmlentities会转化所有的html符号
```

##### 显示客户端IP与服务器IP的代码

```Php
$ip=gethostbyname ("www.google.cn");
echo $ip;
```

##### 遍历一个文件夹下的所有文件和子文件夹

```Php
function my_scandir($dir)
{
  $files=array();
  if(is_dir($dir)){
  	if($handle=opendir($dir)){
  		while(($file=readdir($handle))!==false){
  			if($file != "." && $file != ".."){
              	 		// 判断是否是文件夹
				if(is_dir($dir . "/" . $file)){
					// 递归调用
  					$files[$file]=my_scandir($dir . "/" .$file);
  				}else{
  					$files[]=$dir . "/" . $file;
  				}
  			}
 		 }
  		closedir($handle);
  		return $files;
  	}
  }
}
  print_r(my_scandir(__DIR__ . "/var/www"));
?>
```

##### 不用新变量直接交换现有两个变量的值

```Php
$a = 1;
$b = 2;
list($a, $b) = [$b, $a];
echo $a . $b; // 2 1
```

##### 如何知道有几个参数传入到了一个function?

```php
函数返回传入的参数的个数
func_num_args();
```

##### 求两个日期的差数，例如2007-2-5 ~ 2007-3-6 的日期差数

```Php
$temp = explode('-', '2007-2-5');
$time1 = mktime(0, 0, 0, $temp[1], $temp[2], $temp[0]);
$temp = explode('-', '2007-3-6');
$time2 = mktime(0, 0, 0, $temp[1], $temp[2], $temp[0]);
echo ($time2-$time1)/86400;
```

##### 字符串 "open_door" 转换成 "OpenDoor"、"make_by_id" 转换成 "MakeById"

```php
function change2($str) {  
    $arr = explode('_',$str);  
    foreach($arr as $key=>$val) {  
        $tmp = ucfirst($val);  
        $tarr[] = $tmp;  
    }  
    return implode("",$tarr);  
}  
```

##### 字符串大小写的转换的函数

```php
strtoupper(): 将字符串中的小写字符转变为大写的字符 
strtolower(): 将字符串中大写的字符转变为小写的字符 
ucfirst(): 首字符大写(只针对首字符，不对其他的字符进行操作) 
ucwords():单词首字母大写(只针对每一个单词首字符，不对其他的字符进行操作)
```

##### str_pad() 函数把字符串填充为新的长度

```Php
$str = "Hello World";
echo str_pad($str,30,".:",STR_PAD_BOTH);
// .:.:.:.:.Hello World.:.:.:.:.:
```

##### 2016年11月1日 前6天时间

```php
date("Y-m-d",strtotime('-6 day 1 November 2016'));
```

##### XHTML 与 HTML 之间的差异

```scss
XHTML 元素必须被正确地嵌套。
XHTML 元素必须被关闭。
XHTML 文档必须拥有根元素。
标签名必须用小写字母。
```

##### json 和 jsonp 的区别，返回值的区别

```scss
JSON是数据格式，用在同源异步请求的返回结果。
JSONP是一种跨域请求方式，其原理就是动态生成Script标签，设置src为远端地址，内容为一个js调用。
返回值：
json : {'a':'1','b':'2'}
jsonp: foo({'a':'1','b':'2'});
```

##### FTP DNS SMTP POP3 HTTP HTTPS DHCP DNS SNMP Telnet 协议

```scss
FTP文件传送协议 (File Transfer Protocol 简称FTP) 端口号：21（控制端口），20（数据端口）
DNS（域名解析协议）        端口号：53
SMTP（Simple Mail Transfer Protocal）称为简单邮件传输协议       端口：25
POP的全称是 Post Office Protocol （简称POP3），即邮局协议       端口：110
HTTP（超文本传输协议）     端口：80    范围：0——1023
HTTPS（Secure Hypertext Transfer Protocol 安全超文本传输协议） 端口：443
DHCP（动态主机配置协议）
所有 DHCP 流量都使用用户数据报协议 (UDP)。
从 DHCP 客户端发送到 DHCP 服务器的消息使用 UDP 源端口 68 和 UDP 目标端口 67。
从 DHCP 服务器发送到 DHCP 客户端的消息使用 UDP 源端口 67 和 UDP 目标端口 68
SNMP（简单网络管理协议），    端口号：161
Telnet（运程控制协议），     端口号：23
终端服务远程桌面共享，       端口：3389
```

##### $a = "hello"; $b = &$a;

```Php
$a = "hello";

$b = &$a;

echo $b."\n"; // hello

unset($b);

$b = "world"; 

echo $a."\n";// hello

echo $b."\n";// world

```

##### $var =false; if (empty($var)){echo "null"}

```Php
$var = false;
if (empty($var)){
    echo "null";
}else{
    echo "have value";
}
// null
```

##### strcmp() 比较两个字符串（区分大小写）

```Php
echo strcmp("Hello world!","Hello world!")."\n"; // 两字符串相等 0
echo strcmp("Helloworld","Hello")."\n"; // string1 大于 string2 5
echo strcmp("Hello world!","Hello world! Hello!")."\n"; // string1 小于 string2 -7
```

##### current()函数返回数组中的当前元素的值

```Php
$arr = ['a', 'b', 'c'];

foreach ($arr as $k => $v) {
    echo key($arr) . "=>" . current($arr) ."\n";
    next($arr);
    echo key($arr) . "=>" . current($arr)."\n";
}
// 0=>a
// 1=>b
// 1=>b
// 2=>c
// 2=>c
// =>
```

##### $b = '1d9'; echo ++$b;

```Php
$b = '1d9';
echo ++$b; // 1e0
```












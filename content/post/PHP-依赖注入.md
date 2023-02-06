---
title: PHP 依赖注入
date: 2017-04-06 13:38:41
---

<!--more-->


#### 依赖注入简单说就是通过构造注入，函数调用或者属性的设置来提供组件的赖关系。

下面是一个简单的实例化类：

```Php
<?php
namespace Databases;

class Databases
{
    protected $adapter;
	
  	public function __construct()
    {
        $this->adapter = new MysqlAdapter;
    }
}


class MysqlAdapter {code...}

```

使用依赖注入重构，实现解耦：

```Php
<?php
namespace Database;

class Database
{
    protected $adapter;

    public function __construct(MySqlAdapter $adapter)
    {
        $this->adapter = $adapter;
    }
}


class MysqlAdapter {code...}
```

这样就可以通过外界给予 Database 类的依赖，不是让这个类自己产生依赖的对象。



#### 控制反转

顾名思义，一个系统通过组织控制和对象的完全分离来实现”控制反转”。对于依赖注入，这就意味着通过在系统的其他地方控制和实例化依赖对象，从而实现了解耦。



#### 依赖反转准则

依赖反转准则是面向对象设计准则 S.O.L.I.D 中的 “D” ，倡导 "依赖于抽象而不是具体"，依赖应该是接口或着抽象类，不是固定具体的实现。

```php
<?php
namespace Database;

class Database
{
    protected $adapter;

    public function __construct(AdapterInterface $adapter)
    {
        $this->adapter = $adapter;
    }
}

interface AdapterInterface {}

class MysqlAdapter implements AdapterInterface {code...}
```

这样方法的一个最大的好处是代码扩展性会变得更高。如果项目中决定要迁移一种不同的数据库，只需要写一个实现相应接口的适配器注入进 Database 这个类，根本不需要重构代码。

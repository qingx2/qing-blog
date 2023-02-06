---
title: PHP魔术方法、依赖注入、控制反转容器
date: 2017-09-15 00:31:41
---

### 依赖注入

“注入”，就是把一个实例传递到另一个实例的内部。

### 控制反转容器（Ioc Container）

如果存在如下这样复杂的依赖关系，该如何解决呢？

```php
$a = new A(new B(new D(new F)), new C(new E()));
```

IoC容器（控制反转）就是为了帮助我们解决因复杂的依赖注入而产生混乱的解决方案。

**IoC容器需要满足2个最基本的功能：**

1. *存储定义的类*
2. *实例化类*

<!--more-->

> 大概实现代码如下：

```php
Class Container
  {
  	// 保存定义的类
    private $_definitions;

    // 存储到$_definitions属性中, (类名,创建类)
    public function set($class, $definition)
    {
        $this->_definitions[$class] = $definition;
    }
	
  	// 查找$_definitions属性中的类
    public function get($class, $params = [])
    {
      	if () {
            $definition = $this->_definitions[$class];
            // 调用此类下的定义
            return call_user_func($definition, $params);
        }else {
            echo 'Must callable';die;
        }
    }
}
```



#### 例1. 发送邮件：

```php
class EmailSenderByGmail
{
    private $_name;
    public function __construct($name = '')
    {
        $this->_name = $name;
    }
    public function send()
    {
    }
}
```

如何借用IoC容器操作呢？

```php
$cont = new Container;
$cont->set('EmailSenderByGmail', function($name = ''){
  return new EmailSenderByGmail($name);
});

// 调用
$email_send_gmail = $cont->get('EmailSendByGmail','gmail');

print_r($email_send_gmail);
```



步骤注释：

1. 实例化 `Container` 类，将 `EmailSendByGmail` 实例化后的类保存至 `Container` 里的 `$_definitions` 字段，
2. 通过 `Container` 获取 `EmailSendByGmail` 属性 `_name`



执行得到`_name` 的值：

```php
EmailSendByGmail Object
(
    [_name:EmailSendByGmail:private] => gmail
)
```



#### 例2. 用户注册时，发送邮件场景

*实例化User,同时注入EmailObject*

```php
class User
{
    private $_emailSenderObject;

    public function __construct(EmailSenderByGmail $emailSenderObject)
    {
        $this->_emailSenderObject = $emailSenderObject;
    }
  
  	// 注册时发送邮件功能
    public function register()
    {        
        $this->_emailSenderObject->send();
    }
}
```



#### 使用 `Container` 实例化 `EmailSenderByGmail` 和 `User`

> 可以简单解决一个类依赖其它对象的问题

```php
$container = new Container();

$container->set('EmailSenderByGmail', function ($container, $name = '') {
    return new EmailSenderByGmail($name);
});
$container->set('User', function ($container, $params = []) {
    return new User($container->get($params[0], $params[1]));
});


print_r($container->get('EmailSenderByGmail', ['gmail']));
print_r($container->get('User', ['EmailSenderByGmail', 'gmail']));
```



以上虽然解决了问题，但是又出现另一个令人讨厌的问题：你需要把依赖的全部set进去，再get出来。有什么办法解决吗？用PHP提供的反射API，就可以自动实例化处理。



#### 基于**反射**，改造一下 `Container` 类。

```php
Class Container
{
    public function get($class, $params = [])
    {
        return $this->build($class, $params);
    }

    public function build($class, $params)
    {
        $dependencies = [];
		// 反射 类信息
        $reflection = new ReflectionClass($class);
      	// getConstructor 获取类的构造函数
        $constructor = $reflection->getConstructor();
        if ($constructor !== null) {
          	// 获取构造函数的参数
            foreach ($constructor->getParameters() as $param) {
                $c = $param->getClass();
                if ($c !== null) {
                    $dependencies[] = $this->get($c->getName(), $params);
                }
            }
        }

        foreach ($params as $index => $param) {
            $dependencies[$index] = $param;
        }
		// 从给出的参数创建一个新的类实例
        return $reflection->newInstanceArgs($dependencies);
    }
}
```



还是 `User` ，就可以很简单的进行实例化了。

```php
$container = new Containerl();
$user = $container->get('User);
print_r($user);
```

输出：

```php
User Object
(
    [_emailSenderObject:User:private] => EmailSenderByGmail Object
        (
            [_name:EmailSenderByGmail:private] => 
        )

)
```






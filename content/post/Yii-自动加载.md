---
title: Yii-自动加载
date: 2017-09-28 23:14:08
---

### 入口脚本(web/index.php)

Yii2的**advanced版本**默认是有2个入口的（frontend、backend），但其实只是在使用同一个框架及公共配置让2个server分离工作进行构建网站、API等。

Web服务器每将一个请求转发过来的时候，始终只会进一个入口文件，那就是`index.php`了，这个文件会进行应用的初始化配置、解析路由、处理请求、连接SQL数据库。

说了这么多，来看看入口文件内容: 

<!--more-->

```php
<?php
  // 定义是否处于DEBUG模式(框架初始化时 ./init 选择运行环境会影响此处代码是否会产生)
defined('YII_DEBUG') or define('YII_DEBUG', true);
defined('YII_ENV') or define('YII_ENV', 'dev');

// 载入composer管理依赖包,此处实现了第三方的composer包可以直接调用(因为composer有个autoload.php文件,已经处理好载入类文件的问题)
require(__DIR__ . '/../../vendor/autoload.php');
// 载入基础类文件(核心,没有它就不能算是框架了)
require(__DIR__ . '/../../vendor/yiisoft/yii2/Yii.php');
// 载入公共的全局配置(各应用之间的目录别名就定义在此)
require(__DIR__ . '/../../common/config/bootstrap.php');
// 本server的相关全局配置
require(__DIR__ . '/../config/bootstrap.php');

// 合并所有配置项参数,并调用 Application->run()
$config = yii\helpers\ArrayHelper::merge(
    require(__DIR__ . '/../../common/config/main.php'),
    require(__DIR__ . '/../../common/config/main-local.php'),
    require(__DIR__ . '/../config/main.php'),
    require(__DIR__ . '/../config/main-local.php')
);
(new yii\web\Application($config))->run();

```



### 核心基础类(Yii.php)

说`vendor/yiisoft/yii2/Yii.php`这个类是核心类呢，也不大准确，因为它其实只是一个子类，真正厉害的是它所继承的`BaseYii.php`，先看一下文件内容：

```php
// 此处还不能实现自动加载类,哈哈,所以只好简单粗暴require进来了,万变不离其宗
require(__DIR__ . '/BaseYii.php');

class Yii extends \yii\BaseYii
{
}

spl_autoload_register(['Yii', 'autoload'], true, true);
Yii::$classMap = require(__DIR__ . '/classes.php');
Yii::$container = new yii\di\Container();
```

> BaseYii.php

![](https://ws4.sinaimg.cn/large/006tKfTcgy1fjzp35gnakj30uy0ti7aq.jpg)

下面分析一下`Yii.php`的下面三行代码:



#### 1. spl_autoload_register(['Yii', 'autoload'], true, true);

此处将`yii\BaseYii::autoload`方法注册为自动加载方法，当new一个找不到类的时候，此autoload方法会自动被调用， PHP5.3版本后增加[spl_autoload_register](http://php.net/manual/zh/function.spl-autoload-register.php)函数，使现在的主流框架才有了这么一套自动加载的利器。

来研究一下autoload方法实现了些什么

```php
public static function autoload($className)
    {
  		// 如果 new 未找到的类是 yii2 内置的核心类，便可以快速根据类名找到其对应的路径
        if (isset(static::$classMap[$className])) {
            $classFile = static::$classMap[$className];
            if ($classFile[0] === '@') {
                $classFile = static::getAlias($classFile);
            }
        } elseif (strpos($className, '\\') !== false) { // 自定义类自动载入前替换动作
          	// 将namespace规范的 \ 替换为 / 文件目录地址
            $classFile = static::getAlias('@' . str_replace('\\', '/', $className) . '.php', false);
            if ($classFile === false || !is_file($classFile)) {
                return;
            }
        } else {
            return;
        }

        include($classFile);

        if (YII_DEBUG && !class_exists($className, false) && !interface_exists($className, false) && !trait_exists($className, false)) {
            throw new UnknownClassException("Unable to find '$className' in file: $classFile. Namespace missing?");
        }
    }
```

此处自定义类的自动载入涉及到一个别名的概念:

```php
$classFile = static::getAlias('@' . str_replace('\\', '/', $className) . '.php', false);
```

`BaseYii`文件有个专门存储别名的属性`public static $aliases = ['@yii' => __DIR__];` 回过头看入口文件处引入的`require(__DIR__ . '/../../common/config/bootstrap.php');`，调用`BaseYii`内的`setAlias()`方法将server根目录存入`$aliases`属性:

```php
<?php
Yii::setAlias('@common', dirname(__DIR__));
Yii::setAlias('@frontend', dirname(dirname(__DIR__)) . '/frontend');
Yii::setAlias('@backend', dirname(dirname(__DIR__)) . '/backend');
Yii::setAlias('@console', dirname(dirname(__DIR__)) . '/console');

```





那么`frontend\models\User`通过转换后就会去获得 `frontend`项目下的User文件文件路径，第二个参数决定如果没有找到这个文件是否抛出异常

```php
$classFile = static::getAlias('@frontend/models/User.php', false);
```



#### 2. Yii::$classMap = require(__DIR__ . '/classes.php');

这个目录就很直接了，保存的是框架的核心基础类对应的类文件路径

此处的常量`YII2_PATH`已在`BaseYii`中进行了定义`defined('YII2_PATH') or define('YII2_PATH', __DIR__);`

```php
return [
  'yii\base\Action' => YII2_PATH . '/base/Action.php',
  'yii\base\ActionEvent' => YII2_PATH . '/base/ActionEvent.php',
  'yii\base\ActionFilter' => YII2_PATH . '/base/ActionFilter.php',
  'yii\base\Application' => YII2_PATH . '/base/Application.php',
  'yii\base\ArrayAccessTrait' => YII2_PATH . '/base/ArrayAccessTrait.php',
  'yii\base\Arrayable' => YII2_PATH . '/base/Arrayable.php',
  'yii\base\ArrayableTrait' => YII2_PATH . '/base/ArrayableTrait.php',
  'yii\base\Behavior' => YII2_PATH . '/base/Behavior.php',
  'yii\base\BootstrapInterface' => YII2_PATH . '/base/BootstrapInterface.php',
  'yii\base\Component' => YII2_PATH . '/base/Component.php',
  
  // .......
];
```



#### 3. Yii::$container = new yii\di\Container();

这一步的操作是将实例化依赖注入容器，并赋值给 `3. Yii::$container` ，Yii内部核心对此类使用非常普遍，占有很高的地位！由 `Yii` 继承 `BaseYii::createObject` 里对此类进行更强大更方便的封装，所以一般使用 `Yii::createObject()` 进行创建对象。

> For example

1. T.php

```php
<?php
/**
 * User: surprisepeas
 * Date: 2017/10/18 23:13
 */

namespace backend\components;


class T
{
    public $name = 'This is T name';
}
```

2. Test.php

```php
<?php
/**
 * User: surprisepeas
 * Date: 2017/10/18 23:14
 */

namespace backend\components;


class Test
{
    public $name;
    private $_age;
    private $_t;

    public function __construct($age, T $t)
    {
        $this->_age = $age;
        $this->_t = $t;
    }
}
```

3. ExController

```php
<?php
/**
 * User: surprisepeas
 * Date: 2017/10/18 23:16
 */

namespace console\controllers;


use yii\console\Controller;
use Yii;

class ExController extends Controller
{
    public function actionI()
    {
        $testObject = Yii::createObject([
            'class' => 'backend\components\Test',
            'name'  => 'new test name',
        ],[20]);
        print_r($testObject);

      	// 现在来看 common\config\main.php的配置,有那么点明白了吧..
        $object = Yii::createObject([
            'class' => 'yii\db\Connection',
            'dsn' => 'mysql:host=127.0.0.1;dbname=demo',
            'username' => 'root',
            'password' => '',
            'charset' => 'utf8',
        ]);
        print_r($object);
    }
}
```

console执行命令 `./yii ex/i` 输出结果如下：

```shell
backend\components\Test {#20
  +name: "new test name"
  -_age: 20
  -_t: backend\components\T {#26
    +name: "This is T name"
  }
}

```

具体来分析一下 `Yii::createObject()`

```php
/*
 * 关于$type参数,代码文档说的非常详细,大概翻译如下:
 * - a string: 为字符串时,表示要创建的对象的类名。
 * - a configuration array:为数组时,数组内必须包含一个'class'属性，将被视为对象的类名,其余的属性名的值对将用于初始化对象相应的对象属性。
 * - a PHP callable: 要么是匿名函数,要么是表示类方法的数组,调用应该返		回正在创建的对象的一个新实例。(`[$class or $object, $method]`).
 *
 * $params参数是要创建的对象的构造函数
 */
public static function createObject($type, array $params = [])
    {
        if (is_string($type)) {
            return static::$container->get($type, $params);
        } elseif (is_array($type) && isset($type['class'])) {
            $class = $type['class'];
            unset($type['class']);
            return static::$container->get($class, $params, $type);
        } elseif (is_callable($type, true)) {
            return static::$container->invoke($type, $params);
        } elseif (is_array($type)) {
            throw new InvalidConfigException('Object configuration must be an array containing a "class" element.');
        }

        throw new InvalidConfigException('Unsupported configuration type: ' . gettype($type));
    }
```

从上面代码可以看出，字符串类型还是数组类型的 `$type` 最后其实都是调用 `Yii::$container->get($class)` 实例化类。

再回到上面自己写的例子(T.php Test.php)，其实就是这样：

```php
Yii::$container->get('backend\components\Test', [20], ['name' => 'new test name']);
```

继续往下追踪，看下具体 `get` 到底做了什么 ：

```php
// 判断是否已存在 $class 的定义(单例的实现)
if (isset($this->_singletons[$class])) {
  // singleton
  return $this->_singletons[$class];
  // 如果 Container::_definitions 属性不存在 $class 的定义，即是否定义过以何种形式实例化 $class，哈，这正符合我们的条件，下面我们走到 Container::build 方法
} elseif (!isset($this->_definitions[$class])) {
  return $this->build($class, $params, $config);
}
```

这个时候我们的 `Test` 就成为了这样：

```php
$this->build('backend\components\Test', [20], ['name' => 'new test name']);
```

Container::build 方法是实例化类的方法，对于容器，我们知道，build方法要做的核心应该包括下面3点

1. 获取Test的依赖
2. 解析依赖
3. 实例化对象

我们粗略的看下build方法的实现，看下面代码的注解部分

```php
protected function build($class, $params, $config)
{
    /* @var $reflection ReflectionClass */
    // 获取 $class 的依赖，返回 $class 的反射信息和依赖信息
    list ($reflection, $dependencies) = $this->getDependencies($class);

    // 依赖信息是通过构造方法获取的，所以如果我们通过 get 方法有给构造函数传递参数，理应以我们传递的为准，即覆盖掉 getDependencies 获取的
    foreach ($params as $index => $param) {
        $dependencies[$index] = $param;
    }

    // 解析依赖
    $dependencies = $this->resolveDependencies($dependencies, $reflection);
    if (!$reflection->isInstantiable()) {
        throw new NotInstantiableException($reflection->name);
    }
    // 如果$config为空，即不需要给 $class "注入" 属性和属性值，直接实例化
    if (empty($config)) {
        return $reflection->newInstanceArgs($dependencies);
    }

    // 检查 $class 是否实现了接口 yii\base\Configurable，我们的Test肯定是否了
    if (!empty($dependencies) && $reflection->implementsInterface('yii\base\Configurable')) {
        // set $config as the last parameter (existing one will be overwritten)
        $dependencies[count($dependencies) - 1] = $config;
        return $reflection->newInstanceArgs($dependencies);
    } else {
        // 实例化 $class，并"注入"属性，最后返回我们的对象 $object
        $object = $reflection->newInstanceArgs($dependencies);
        foreach ($config as $name => $value) {
            $object->$name = $value;
        }
        return $object;
    }
}
```

整个过程我们明白了，但是 build方法内获取依赖以及解析依赖部分我们有必要看一下，因为这涉及到一个新的源码类 yii\di\Instance 。

首先我们看获取类的依赖 Container::getDependencies方法，该方法的核心实现我们并不陌生。在 [php之控制反转容器（Ioc Container）](http://xn--zwtw53i.com/2017/09/15/PHP%E9%AD%94%E6%9C%AF%E6%96%B9%E6%B3%95%E3%80%81%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5%E3%80%81%E6%8E%A7%E5%88%B6%E5%8F%8D%E8%BD%AC/)一文中最后实现Container容器部分的操作非常雷同。

跟我们手动实现的Container容器相比，注意下面标记的4个地方

```php

protected function getDependencies($class)
{
    // ①、判断 Container::_reflections 属性是否有保存 $class 的反射信息，如果有就直接返回，不用再解析，毕竟获取类的反射信息也是要消耗时间
    if (isset($this->_reflections[$class])) {
        return [$this->_reflections[$class], $this->_dependencies[$class]];
    }

    $dependencies = [];
    $reflection = new ReflectionClass($class);

    $constructor = $reflection->getConstructor();
    if ($constructor !== null) {
        foreach ($constructor->getParameters() as $param) {
            // ②、获取构造函数的参数时，通过 isDefaultValueAvailable 方法判断参数的默认值
            if ($param->isDefaultValueAvailable()) {
                $dependencies[] = $param->getDefaultValue();
            } else {
                $c = $param->getClass();
                // ③、调用 yii\di\Instance::of 方法处理依赖的类名，Instance::of 方法返回 Instace 类的实例
                $dependencies[] = Instance::of($c === null ? null : $c->getName());
            }
        }
    }
    // ④、将反射信息和依赖信息分别保存在 Container::_reflections和Container::_dependencies属性，防止重复实例化该类时重复解析反射信息和依赖信息
    $this->_reflections[$class] = $reflection;
    $this->_dependencies[$class] = $dependencies;

    return [$reflection, $dependencies];
}
```

第③点的 yii\di\Instance，这个类起什么作用呢？

我们准备先说一说依赖解析方法 Container::resolveDependencies 再聊一聊这个事。

```php
protected function resolveDependencies($dependencies, $reflection = null)
{
    foreach ($dependencies as $index => $dependency) {
        if ($dependency instanceof Instance) {
            if ($dependency->id !== null) {
                $dependencies[$index] = $this->get($dependency->id);
            } elseif ($reflection !== null) {
                $name = $reflection->getConstructor()->getParameters()[$index]->getName();
                $class = $reflection->getName();
                throw new InvalidConfigException("Missing required parameter \"$name\" when instantiating \"$class\".");
            }
        }
    }
    return $dependencies;
}
```

这个方法很简单，在该方法中，循环 $class 的依赖信息，判断每一个参数，如果是 yii\di\Instance的实例并且 yii\di\Instace::id属性非空时，再次通过 Container::get方法获取对象。

这是什么意思？

我们用 Test 这个例子来理一下头绪。

首先看一下此时 Test 的依赖信息的数据结构，即 Container::resolveDependencies 方法的第一个参数， $dependencies

```php
Array
(
    [0] => 20
    [1] => yii\di\Instance Object
        (
            [id] => backend\components\T
        )

)
```

这样就清晰多了，如此我们便不难理解 resolveDependencies 方法中再次用 $this->get($dependency->id) 的作用，实例化依赖对象嘛。

前前后后合计着 yii\di\Instance 只是临时保存依赖的类名或者接口名而已，这样一来，Container::getDependencies 方法的第③点我们也就明白了。

通常，在配置依赖注入容器时，我们可以通过 Instance 来引用类名、接口名或者别名（并非是我们上文介绍的alias别名，这里只是一个名字而已，比如"className"），随后会再次被 Container容器解析为实际的对象。

比如

```php
$container = new \yii\di\Container;
$container->set('cache', [
    'class' => 'yii\caching\DbCache',
    'db' => \yii\di\Instance::of('db')
]);
```


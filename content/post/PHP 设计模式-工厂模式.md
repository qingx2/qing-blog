---
title: "PHP 设计模式 工厂模式"
author: "杰枫 Jeff"
tags: ["PHP", "设计模式"]
date: 2018-10-16T16:11:08+08:00
draft: false
---

### 那么何为工厂模式？

　　定义一个创建对象的接口，但是让子类去实例化具体类。工厂方法模式让类的实例化延迟到子类中。

  　*工厂模式有一个关键的构造，根据一般原则命名为 Factory 的静态方法，然而这只是一种原则，虽然工厂方法可以任意命名这个静态还可以接受任意数据的参数，必须返回一个对象。*

<!--more-->

### 为什么要用工厂模式？

　　很多没接触过工厂模式的人会不禁问，为啥我要费那么大的劲儿去构造工厂类去创建对象呢？不去套用那些易维护，可扩展之类的话，我们可以考虑这样一个简单的问题。如果项目中，我们通过一个类创建对象。在快完成或者已经完成，要扩展功能的时候，发现原来的类类名不是很合适或者发现类需要添加构造函数参数才能实现功能扩展。我都通过这个类创建了一大堆对象实例了啊，难道我还要一个一个去改不成？我们现在才感受到了 “高内聚低耦合” 的博大精深。没问题，工厂方法可以解决这个问题。

​	再考虑一下，我要连接数据库，在 PHP 里面就有好几种方法，mysql 扩展，mysqli 扩展，PDO 扩展。我就是想要一个对象用来以后的操作，具体要哪个，视情况而定喽。既然你们都是连接数据库的操作，你们就应该拥有相同的功能，建立连接，查询，断开连接...（此处显示接口的重要性）。总而言之，这几种方法应该 “团结一致，一致对外”。如何实现呢？利用工厂模式。

### 工厂模式如何实现？

​	相对于单例模式，上面我们提供了足够的信息，工厂类，工厂类里面的静态方法。静态方法里面 new 一下需要创建的对象实例就搞定了。当然至于考虑上面的第二个问题，根据工厂类静态方法的参数，我们简单做个判断就好了。管你用 `if..else..` 还是 `switch..case..`，能快速高效完成判断该创建哪个类的工作就好了。最后，一定要记得，**工厂类静态方法只返回一个对象**。不是两个，更不是三个。



> 基本的工厂类

```PHP
//要创建对象实例的类
class MyObject{
}

 //工厂类
class MyFactory{
	public static function factory(){
        return new MyObject():
    }
}


$instance=MyFactory::factory();
```



>  一个稍微复杂的工厂模式
>
> 需要注意同名构造函数的问题，不能是工厂类名为 `Factory` ，静态方法名为 `factory()` 

```PHP
interface Transport{
    public function go();

}

class Bus implements Transport{
    public function go(){
        echo "bus每一站都要停";
    }
}

class Car implements Transport{
    public function go(){
        echo "car跑的飞快";
    }
}

class Bike implements Transport{
    public function go(){
        echo "bike比较慢";
    }
}

class transFactory{
    public static function factory($transport)
    {
        
        switch ($transport) {
            case 'bus':
                return new Bus();
                break;

            case 'car':
                return new Car();
                break;
            case 'bike':
                return new Bike();
                break;
        }
    }
}


$transport=transFactory::factory('car');
$transport->go();
```
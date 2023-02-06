---
title: "PHP 设计模式 观察者模式"
author: "杰枫 Jeff"
tags: ["PHP", "设计模式"]
date: 2018-10-16T16:10:38+08:00
draft: false
---

​	    这是我写的《php 模式设计》的第五篇。前面的四篇在不断学习不断加深认识，到了今天再看观察者模式，觉得非常容易理解。这也许就是我们积少成多的结果吧。希望还是能够不断进步。

   	　开篇还是从名字说起，“观察者模式” 的观察者三个字信息量很大。玩过很多网络游戏的童鞋们应该知道，即便是斗地主，除了玩家，还有一个角色叫 “观察者 "。在我们今天他谈论的模式设计中，观察者也是如此。首先，要有一个 “主题”。只有有了一个主题，观察者才能搬着小板凳儿聚在一堆。其次，观察者还必须要有自己的操作。否则你聚在一堆儿没事做也没什么意义。

<!--more-->

　　　从面向过程的角度来看，首先是观察者向主题**注册**，注册完之后，主题再**通知**观察者做出相应的**操作**，整个事情就完了。

　　　从面向对象的角度来看，主题提供注册和通知的接口，观察者提供自身操作的接口。（这些观察者拥有一个同一个接口。）观察者利用主题的接口向主题注册，而主题利用观察者接口通知观察者。耦合度相当之低。

​         如何实现观察者注册？通过前面的注册者模式很容易给我们提供思路，把这些对象加到一棵注册树上就好了嘛。如何通知？这就更简单了，对注册树进行遍历，让每个对象实现其接口提供的操作。



```php
<?php
// 主题接口
interface Subject{
    public function register(Observer $observer);
    public function notify();
}
// 观察者接口
interface Observer{
    public function watch();
}
// 主题
class Action implements Subject{
     public $_observers=array();
     public function register(Observer $observer){
         $this->_observers[]=$observer;
     }

     public function notify(){
         foreach ($this->_observers as $observer) {
             $observer->watch();
         }

     }
 }

// 观察者
class Cat implements Observer{
     public function watch(){
         echo "Cat watches TV<hr/>";
     }
 } 
 class Dog implements Observer{
     public function watch(){
         echo "Dog watches TV<hr/>";
     }
 } 
 class People implements Observer{
     public function watch(){
         echo "People watches TV<hr/>";
     }
 }

// 应用实例
$action=new Action();
$action->register(new Cat());
$action->register(new People());
$action->register(new Dog());
$action->notify();
```

　　　所谓模式，更多的是一种想法，完全没必要拘泥于代码细节。观察者模式更多体现了两个独立的类利用接口完成一件本应该很复杂的事情。不利用主题类的话，我们还需要不断循环创建实例，执行操作。而现在只需要创建实例就好，执行操作的事儿只需要调用一次通知的方法就好啦。

　　　从开始的单例模式我一步步考虑如何实现代码，到现在大部分实现代码一句带过，实际上是建立在前面不断积累的基础上。真心感觉通过不断学习设计模式能很大加深对面向对象编程的思考。当然纸上谈兵还是要不得的，最好还是投入更多的练习中去吧~~·

　　相关文章  《[使用观察者模式处理异常信息](http://www.cnblogs.com/DeanChopper/p/4726773.html)》
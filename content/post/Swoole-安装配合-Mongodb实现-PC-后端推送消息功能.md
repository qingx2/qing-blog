---
title: Swoole 实现消息发送功能
date: 2017-05-22 23:54:53
---

#### 导语

- [Swoole](http://www.swoole.com) 是 PHP 的一个扩展，使用纯C语言编写，Swoole底层内置了异步非阻塞、多线程的网络IO服务器。PHP程序员仅需处理事件回调即可，无需关心底层。
- 官网提供了[PHP语言的异步多线程服务器](https://wiki.swoole.com/wiki/page/p-server.html)，[异步TCP/UDP网络客户端](https://wiki.swoole.com/wiki/page/p-client.html)，[异步MySQL](https://wiki.swoole.com/wiki/page/517.html)，[异步Redis](https://wiki.swoole.com/wiki/page/p-redis.html)，[数据库连接池](https://github.com/swoole/framework/blob/master/tests/async_mysql.php)，[AsyncTask](https://wiki.swoole.com/wiki/page/134.html)，[消息队列](http://wiki.swoole.com/wiki/page/289.html)，[毫秒定时器](https://wiki.swoole.com/wiki/page/244.html)，[异步文件读写](https://wiki.swoole.com/wiki/page/183.html)，[异步DNS查询](https://wiki.swoole.com/wiki/page/186.html)。 Swoole内置了[Http/WebSocket服务器端](https://wiki.swoole.com/wiki/page/326.html)/[客户端](https://wiki.swoole.com/wiki/page/p-http_client.html)、[Http2.0服务器端](https://wiki.swoole.com/wiki/page/326.html)。

<!--more-->

#### 安装Swoole

- 下载地址：[Downloads](https://github.com/swoole/swoole-src/releases)

- 解包，进入swoole-src 目录执行

  ```powershell
  ~ ᐅ phpize
  ~ ᐅ ./configure
  ~ ᐅ make 
  ~ ᐅ sudo make install
  ```

- 打开 `php.ini` 文件添加扩展（具体根据自己使用的 PHP 路径）：

  > vi /usr/local/etc/php/7.0/php.ini

  ```powershell
  extension=swoole.so
  ```

- 重启 PHP-FPM

  > sudo /usr/local/Cellar/php70/7.0.18_10/sbin/php70-fpm restart

- 查看Swoole 扩展是否已安装成功

  > php -m

  ​

#### 运行 Server

- 创建一个 `server.php` 文件，用来启动 Swoole 的服务

  ```php
  <?php

  // 创建 server 并监听9300端口
  $server = new \swoole_server("127.0.0.1", 9300);

  //监听连接进入事件
  $server->on('connect', function ($server, $fd) {  
      echo "Client: Connect.\n";
  });

  //监听数据发送事件
  $server->on('receive', function ($server, $fd, $from_id, $data) {
      $server->send($fd, "Server: ".$data);
  });

  //监听连接关闭事件
  $server->on('close', function ($server, $fd) {
      echo "Client: Close.\n";
  });

  //启动服务器
  $server->start(); 
  ```

  运行 `server.php` 文件就会发现开始监听9300端口了

  > php server.php

  另开一个终端执行远程登录输入 ‘hello world’

  > telnet 127.0.0.1 9300

  ```powershell
  Trying 127.0.0.1...
  Connected to localhost.
  Escape character is '^]'.
  hello world
  Server: hello world
  ```

- 创建 `client.php` 文件，建立 TCP客户端请求发送数据

  ```php
  <?php
  /**
   * client
   */

  function writeMsg($message = "Hello Everybody")
  {
      $host = "127.0.0.1";
      $port = 9300;
      // $client = new Swoole\Client(SWOOLE_TCP | SWOOLE_ASYNC | SWOOLE_SSL);
      $client = new \swoole_client(SWOOLE_SOCK_TCP);
      // 连接远程服务器：bool $swoole_client->connect(string $host, int $port, float $timeout = 0.1, int $flag = 0)
      if (!$client->connect($host, $port, 1)) {
          echo "Error: {$client->errMsg}[{$client->errCode}]\n";
      }

    	/*
    	转码发送发送给服务端
    	$data = array(
              "url" =>  "http://127.0.0.1/send_mail" ,
              "param" => array(
                  "username"=>'test',
                  "password" => 'test'
                  )
              );
      $json_data = json_encode($data);
      $client->send($json_data);
    	*/
    
      $client->send($message);

      // 从服务器接收数据
      $data = $client->recv();
      if (!$data) {
          die("recv failed.");
      }
      echo $data;
      // 关闭连接
      $client->close();
  }

  writeMsg();	
  ```

- 调用客户端
> php client.php

- 服务端会返回

```powershell
Swoole: Hello Everybody
```



***本文只是个 demo，client.php 课根据自己的业务需求增加业务代码就行，简单流程就是这样***



再推荐一个[Workerman](http://www.workerman.net)（是一款**纯PHP开发**的**开源**高性能的PHP socket 服务器框架），用法更加简单，只需要一个 pcntl 的网络扩展 + 引入类文件就可以使用。



- [Workerman - 高性能的PHP socket框架](https://surprisepeas.github.io/2017/04/20/Workerman-高性能的PHP-socket框架/)


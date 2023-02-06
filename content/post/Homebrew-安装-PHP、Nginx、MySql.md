---
title: Homebrew 安装 PHP、Nginx、MySql
date: 2017-04-19 09:32:37
---

参考 [全新安装 Mac OS Sierra (10.12) 并使用 HomeBrew 安装 ZSH + MNMP (Mac + Nginx + MySQL + PHP) 开发环境](https://laravel-china.org/topics/3129/new-installation-mac-os-sierra-1012-and-use-homebrew-to-install-zsh-mnmp-mac-nginx-mysql-php-development-environment)

<!--more-->

#### 安装 Mysql

查找 mysql

```powershell
➜  ~ brew search mysql
automysqlbackup                      mysql-connector-c
homebrew/php/php53-mysqlnd_ms        mysql-connector-c++
homebrew/php/php54-mysqlnd_ms        mysql-sandbox
homebrew/php/php55-mysqlnd_ms        mysql-search-replace
homebrew/php/php56-mysqlnd_ms        mysql-utilities
mysql                                mysql@5.5
mysql++                              mysql@5.6
mysql-cluster                        mysqltuner
Caskroom/cask/mysql-connector-python
Caskroom/cask/mysql-shell
Caskroom/cask/mysql-utilities
Caskroom/cask/mysqlworkbench
Caskroom/cask/navicat-for-mysql
Caskroom/cask/sqlpro-for-mysql
```

查看版本信息

```powershell
➜  ~ brew info mysql
mysql: stable 5.7.17 (bottled)
```

执行安装

```powershell
➜  ~ brew install mysql
# 略...
# 安装完成后, 倒数第三行:
# We've installed your MySQL database without a root password. To secure it run:
#    mysql_secure_installation

# To connect run:
#    mysql -uroot
```

上面安装成功后提示的意思是说, 你安装的 MySql 没有密码, 执行`mysql_secure_installation` 进行设置密码, 其中会提示很多信息让你设置, 根据提示自己设置就行了。

进入 MySql，看看是否已经成功

```powershell
➜  ~ mysql -uroot -p
```

查看数据库

```mysql
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)
```

OK! MySql已经安装成功了。

```Mysql
# 执行 exit 退出 MySql
mysql> exit
```



#### 安装 PHP

搜索PHP70版本`brew search php` 如果有提示信息，那就需要按照提示来添加PHP 扩展：

```powershell
➜  ~ brew tap homebrew/dupes
➜  ~ brew tap josegonzalez/homebrew-php
```

此时就可以正常搜索安装 PHP 了，以下参数可以使用 `brew options php70` 来根据自己需求添加安装

```powershell
➜  ~ brew install php70 --with-debug --with-gmp --with-homebrew-curl --with-homebrew-libressl --with-homebrew-libxml2 --with-homebrew-libxslt --with-imap --with-libmysql --with-mssql --with-pear
```

安装完成后，覆盖 macOS 自带的 PATH 路径，执行以下命令将路径添加`~/.bash_profile`文件:

```powershell
➜  ~ echo 'export PATH="$(brew --prefix php70)/bin:$PATH"' >> ~/.bash_profile
➜  ~ echo 'export PATH="$(brew --prefix php70)/sbin:$PATH"' >> ~/.bash_profile
➜  ~ echo 'export PATH="/usr/local/bin:/usr/local/sbib:$PATH"' >> ~/.bash_profile
```

添加完成后查看版本信息, 出现以下提示说明已正常工作:

```powershell
➜  ~ php -v
PHP 7.0.18 (cli) (built: Apr 28 2017 10:34:29) ( NTS DEBUG )
Copyright (c) 1997-2017 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2017 Zend Technologies
➜  ~ php-fpm -v
PHP 7.0.18 (fpm-fcgi) (built: Apr 28 2017 10:34:35) (DEBUG)
Copyright (c) 1997-2017 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2017 Zend Technologies
```

启动 PHP-FPM:

```powershell
# 先将系统自带 php-fpm 杀死
➜  ~ killall php-fpm
# 启动7.0版本的 php-fpm
➜  ~ sudo php-fpm -D
```

#### 安装 Nginx

```powershell
➜  ~ brew install nginx
Updating Homebrew...
==> Downloading https://homebrew.bintray.com/bottles/nginx-1.10.3.sierra.bottle.tar.gz
Already downloaded: /Users/surprisepeas/Library/Caches/Homebrew/nginx-1.10.3.sierra.bottle.tar.gz
==> Pouring nginx-1.10.3.sierra.bottle.tar.gz
==> Using the sandbox
==> Caveats
Docroot is: /usr/local/var/www

The default port has been set in /usr/local/etc/nginx/nginx.conf to 8080 so that
nginx can run without sudo.

nginx will load all files in /usr/local/etc/nginx/servers/.

To have launchd start nginx now and restart at login:
  brew services start nginx
Or, if you don't want/need a background service you can just run:
  nginx
==> Summary
🍺  /usr/local/Cellar/nginx/1.10.3: 8 files, 980.9KB
➜  ~ nginx -v
nginx version: nginx/1.10.3
```

启动 Nginx:

```powershell
➜  ~ sudo nginx
#      	  重新加载配置|重启  |停止|退出
➜  ~ nginx -s reload|reopen|stop|quit
➜  ~ nginx -s stop
```

重置nginx.conf文件

```powershell
nginx -c /usr/local/etc/nginx/nginx.conf
```



 配置 `/usr/local/etc/nginx/nginx.conf` :

```powershell
worker_processes  1;

error_log   /usr/local/var/log/nginx/error.log debug;
pid        /usr/local/var/run/nginx.pid;

events {
    worker_connections  256;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /usr/local/var/log/nginx/access.log  main;

    sendfile        on;
    keepalive_timeout  65;
    port_in_redirect off;

    include /usr/local/etc/nginx/servers/*;
}
```

添加 `php-fpm` 文件,

```powershell
➜  ~ vi /usr/local/etc/nginx/php-fpm
```



```powershell
location ~ \.php$ {
  try_files                   $uri = 404;
  fastcgi_pass                127.0.0.1:9000;
  fastcgi_index               index.php;
  fastcgi_intercept_errors    on;
  include /usr/local/etc/nginx/fastcgi.conf;
}
```

在 `/usr/local/var/www` 目录下创建项目文件, 

```powershell
➜  ~ vi /usr/local/var/www/info.php
# 在 info.php 文件写入
<?php info(); ?>
```

创建配置站点目录的配置文件:

```powershell
➜  ~ vi /usr/local/etc/nginx/servers/default.conf
```

在 `default.conf` 文件中写入:

```Shell
server {
    listen       80;
    server_name  localhost;
    root         /usr/local/var/www/default;

    access_log  /usr/local/var/log/nginx/default.access.log  main;

    location / {
        index  index.html index.htm index.php;
        autoindex   on;
        include     /usr/local/etc/nginx/php-fpm;
    }

    error_page  404     /404.html;
    error_page  403     /403.html;
}
```

重启 nginx服务:

```powershell
➜  ~ sudo nginx -s reload
```

打开浏览器键入地址 http://localhost/info.php ![web](http://omixc2ggv.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-04-28%2014.44.33.png)


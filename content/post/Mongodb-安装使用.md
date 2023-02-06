---
title: 'Mongodb 安装使用'
date: 2017-05-22 23:22:08
---

*Mac 环境*



#### 安装Mongodb

```javascript
brew install mongodb
```

安装完以后可以看见有很明显的提示:

```javascript
brew services start mongodb
Or, if you don't want/need a background service you can just run:
  mongod --config /usr/local/etc/mongod.conf
```

为了方便使用, 将其设置为全局变量

```javascript
// 在 ~/.bash_profile 添加PATH路径信息
➜  ~ sudo vi ~/.bash_profile

export PATH="/usr/local/Cellar/mongodb/3.4.3/bin:$PATH"
```

#### 启动 mongodb

```javascript
sudo mongod --dbpath=/usr/local/var/mongodb
```

启动成功后末尾会有如下信息

```javascript
2017-05-22T23:18:24.487+0800 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/usr/local/var/mongodb/diagnostic.data'
2017-05-22T23:18:24.489+0800 I NETWORK  [thread1] waiting for connections on port 27017
```

说明已经启动成功了， 如果不成功，可能就是数据库路径不正确，仔细看一下提示。启动的时候可定义数据库路径。

shell 窗口不要关闭，让其先一直启动连接着，再开一个 shell窗口，输入 `mongo` 就可以进入管理台管理 mongodb 数据库了;

```javascript
➜  ~ mongo
MongoDB shell version v3.4.3
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.4.3
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
	http://docs.mongodb.org/
Questions? Try the support group
	http://groups.google.com/group/mongodb-user
Server has startup warnings:
2017-05-22T23:18:24.477+0800 I CONTROL  [initandlisten]
2017-05-22T23:18:24.478+0800 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2017-05-22T23:18:24.478+0800 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2017-05-22T23:18:24.478+0800 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2017-05-22T23:18:24.478+0800 I CONTROL  [initandlisten]
> db
test
```

使用数据库，有就直接进入，没有会自动创建

```
use databasename
```

创建数据表

```JavaScript
db.createCollection("Account")
```



ok.. have fun ~






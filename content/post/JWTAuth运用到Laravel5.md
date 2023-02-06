---
title: JWTAuth运用到Laravel5
date: 2017-07-14 14:56:37
---

## JSON Web Token Authentication for Laravel & Lumen

​	现在项目基本全是前后端分离的开发模式，特别是在使用Angular这种前端框架来构建单页面应用程序时会发现配合 `angular-locker` 实现存储令牌等方面更加得心应手，So 此次项目决定用jwt-auth建立一套API规范来练练手。

​	jwt-auth使用JSON Web令牌（[spec](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)）在Laravel中提供了一种简单的身份验证手段，是一个非常轻巧的规范。这个规范允许我们使用JWT在用户和服务器之间传递安全可靠的信息。

<!--more-->


### JWT定义及其组成

**一个JWT实际上就是一个字符串，它由三部分组成，头部、载荷与签名。**

#### **载荷（Payload）**

我们先将用户认证的操作描述成一个JSON对象。其中添加了一些其他的信息，帮助今后收到这个JWT的服务器理解这个JWT。

```json
{
    "sub": "1",
    "iss": "http://localhost:8000/auth/login",
    "iat": 1451888119,
    "exp": 1454516119,
    "nbf": 1451888119,
    "jti": "37c107e4609ddbcc9c096ea5ee76c667"
}
```

这里面的6个字段都是由JWT的标准所定义的。

- sub: 该JWT所面向的用户
- iss: 该JWT的签发者
- iat(issued at): 在什么时候签发的token
- exp(expires): token什么时候过期
- nbf(not before)：token在此时间之前不能被接收处理
- jti：JWT ID为web token提供唯一标识

[详细定义](https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32)

JWT的载荷形式是以base64编码得到的,base64编码上面的JSON可得到如下:

```basic
eyJzdWIiOiIxIiwiaXNzIjoiaHR0cDpcL1wvbG9jYWx
ob3N0OjgwMDFcL2F1dGhcL2xvZ2luIiwiaWF0IjoxNDUxODg4MTE5LCJleHAiOjE0NTQ1MTYxMTksIm5iZiI6MTQ1MTg4OD
ExOSwianRpIjoiMzdjMTA3ZTQ2MDlkZGJjYzljMDk2ZWE1ZWU3NmM2NjcifQ
```

可使用Node.js进行解码翻译出如下格式:

```javascript
var base64url = require('base64url')
var header = {
    "from_user": "B",
    "target_user": "A"
}
console.log(base64url(JSON.stringify(header)))
```



#### 头部（Header）

头部用于描述关于该JWT的最基本的信息，例如其类型以及签名所用的算法等。这也可以被表示成一个JSON对象：

```json
{
  "typ": "JWT",
  "alg": "HS256"
}
```

表示的意思是：这是一个JWT，使用HS256签名算法。

base64编码后：

```basic
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9
```



#### 签名

将上面的两个编码后的字符串都用句号.连接在一起（头部在前），就形成了：

```basic
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxIiwiaXNzIjoiaHR0cDpcL1wvbG9jYWx
ob3N0OjgwMDFcL2F1dGhcL2xvZ2luIiwiaWF0IjoxNDUxODg4MTE5LCJleHAiOjE0NTQ1MTYxMTksIm5iZiI6MTQ1MTg4OD
ExOSwianRpIjoiMzdjMTA3ZTQ2MDlkZGJjYzljMDk2ZWE1ZWU3NmM2NjcifQ
```

最后，我们将上面拼接完的字符串用HS256算法进行加密。在加密的时候，我们还需要提供一个密钥（secret）:

```javascript
HMACSHA256(
    base64UrlEncode(header) + "." +
    base64UrlEncode(payload),
    secret
)
```

得到加密内容:

```basic
wyoQ95RjAyQ2FF3aj8EvCSaUmeP0KUqcCJDENNfnaT4
```

上面这条也叫签名。所以把这条也拼接在上面组合好的后面就得到了一条完整的JWT：

```basic
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxIiwiaXNzIjoiaHR0cDpcL1wvbG9jYWx
ob3N0OjgwMDFcL2F1dGhcL2xvZ2luIiwiaWF0IjoxNDUxODg4MTE5LCJleHAiOjE0NTQ1MTYxMTksIm5iZiI6MTQ1MTg4OD
ExOSwianRpIjoiMzdjMTA3ZTQ2MDlkZGJjYzljMDk2ZWE1ZWU3NmM2NjcifQ.wyoQ95RjAyQ2FF3aj8EvCSaUmeP0KUqcCJDENNfnaT4
```



### 运用到Laravel5



**安装**

[Wiki](https://github.com/tymondesigns/jwt-auth/wiki/Installation)

```shell
"require": {
    "tymon/jwt-auth": "0.5.*"
}
```

Laravel  `app.php` 配置项里添加

```php
// providers
'Tymon\JWTAuth\Providers\JWTAuthServiceProvider'

// aliases
'JWTAuth' => 'Tymon\JWTAuth\Facades\JWTAuth'
'JWTFactory' => 'Tymon\JWTAuth\Facades\JWTFactory'
```

生成配置文件执行命令:

```shell
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\JWTAuthServiceProvider"
# 这一步会在config目录下生成jwt.php配置文件
```

执行命令生成密钥，用于签署我们的令牌

```shell
php artisan jwt:generate

# 得到如下提示
jwt-auth secret [zoKG3UnmTd5qTX5A7Kc6RECIdqDmrbFq] set successfully.
```



### 文件配置

```php
// 刚才执行的jwt:generate命令就是生成了一个随机密钥
'secret' => env('JWT_SECRET', 'changeme'),

// Token time live 令牌有效时间,默认一小时
'ttl' => 60,

// 设置刷新令牌时间,让用户重新登录,默认2周
'refresh_ttl' => 20160,

// 加密算法
'algo' => 'HS256',

// 用户DataModel
'user' => 'App\User',

// 用户标识符
'identifier' => 'id',

// 
'required_claims' => ['iss', 'iat', 'exp', 'nbf', 'sub', 'jti'],

// 黑名单限制功能,默认开启。如果开销小的话可以设置为false，不会有所限制
'blacklist_enabled' => env('JWT_BLACKLIST_ENABLED', true),


// 
'providers' => [
  
 ]
```












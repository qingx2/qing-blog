---
title: 'Laravel语法提示PHPStorm '
date: 2017-05-30 16:21:50
tags: [Laravel, PHP]
---

#### 添加 composer 依赖包

在 `composer.json` 文件里添加依赖包信息

> "barryvdh/laravel-ide-helper":"dev-master"
>
> "php artisan ide-helper:generate"

```json
"require": {
  	// other composer ...

	"barryvdh/laravel-ide-helper":"dev-master"
}
"scripts": {
  "post-update-cmd": [
	// other cmd..
    // 以后执行 composer update 的时候会自动执行此扩展包的任务
    "php artisan ide-helper:generate"
   ]
},
```

#### 添加service provider 服务

在你的Laravel项目 `config/app.php` 于 providers 添加如下代码注册为服务提供者，使其在每个环境下都加载进来：

```php
Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class,
```

保存退出，执行 `composer update` 下载依赖包并会自动执行扩展包任务
---
title: 使用GitHub写Blog
date: 2017-03-16 21:00:24
---
### 创建GitHub Pages
* 新建一个GitHub的仓库, 仓库名称必须设置为`$username.github.io`     (比如我的GitHub用户名为`SurprisePeas`, Repository填写`SurprisePeas.github.io`)

![](http://omixc2ggv.bkt.clouddn.com/14884447927307.jpg)

<!--more-->

### 环境准备( 安装 Node.js Git )

**安装Git Node**

```
brew install git
brew install node
```

> 查看已安装的开源软件包、安装位置

```
brew list
brew list node
```

**安装配置Hexo 搭建博客的核心**

感兴趣想深入了解Hexo的同学可以查看[Hexo官网](https://hexo.io/)解锁更多玩法。

![](http://omixc2ggv.bkt.clouddn.com/14884573951837.jpg)


> 安装Hexo

```
npm install hexo-cli -g
```

> 安装好hexo以后，在终端输入：`hexo` , 若出现下图，说明安装成功:

![](http://omixc2ggv.bkt.clouddn.com/14884573018737.jpg)

> 初始化创建博客(blog)

```
// 进入目录
cd <path>

// 执行init初始化并创建blog项目文件
hexo init <blog>

// 进入demo博客目录
cd blog

// 根据dependencies配置安装所有的依赖包
npm install
```

* 初始化后的目录结构如图:

![](http://omixc2ggv.bkt.clouddn.com/14884586539587.jpg)

 **配置Blog项目**

> 1.修改_config.yml文件参数信息

```
vi _config.yml
```

> 2.修改网站基础相关信息

```
title: Blog Title
subtitle: 副标题
description: 描述
author: 作者
language: zh-CN
timezone: Asia/Shanghai

## 注意！配置项里的每一个: 后面都有一个空格
```

> 下拉到底，配置部署与GitHub建立连接

```
deploy:
  type: git
  repo: https://github.com/SurprisePeas/SurprisePeas.github.io.git
  branch: master
```
`repo` 是之前GitHub创建好的仓库Repository

`branch` 填写的是GitHub上默认的master分支

![](http://omixc2ggv.bkt.clouddn.com/14884596006556.jpg)

> 新建一篇文章(.MD格式)

```
hexo n "First Blood"
```

会生成如下图:

![](http://omixc2ggv.bkt.clouddn.com/14884598367964.jpg)

新建的MD文件都会生成在 `source/_posts` 下存放，编辑 `First-Blood.md` 文件：

```
vi source/_posts/First-Blood.md
```

会看到这样内容，写下“新建一篇文章”: 

![](http://omixc2ggv.bkt.clouddn.com/14884601339018.jpg)

保存退出后，本地发布查看:

```
# 编译生成html
hexo g
# 本地
hexo s
```

如下图:

![](http://omixc2ggv.bkt.clouddn.com/14884603397042.jpg)

 在浏览器访问 `http://localhost:4000` 

 ![本地server预览](http://omixc2ggv.bkt.clouddn.com/14884604959301.jpg)


 当然也可以将.MD格式的文件直接复制到 `source/_posts` 这个目录。

 **接下来生成静态文件,就可以将本地的文章通过GitHub发布到网上了**

```
hexo g
hexo d
```

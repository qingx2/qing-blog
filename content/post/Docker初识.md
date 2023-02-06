---
title: "Docker初识"
tags: ["tagA", "tagB"]
date: 2018-08-05T10:33:19+08:00
draft: false
---

<!--more-->

### 使用容器

如何管理我们的容器？

当你在docker容器中运行和管理你的应用程序，我们会展示如何管理这些容器。了解如何检查、监控和管理容器。

阅读[使用容器](http://www.widuu.com/docker/userguide/usingdocker.html)

### 使用docker镜像

我是如何创建、访问和分享我自己的容器呢？

当你学会如何使用docker的时候，是时候进行下一步来学习如何在 Docker 中构建你自己应用程序镜像。

阅读[使用docker镜像](http://www.widuu.com/docker/userguide/dockerimages.html)

### 容器连接

到这里我们学会了如何在 Docker 容器中构建一个单独的应用程序。而现在我们要学习如何将多个容器连接在一起构建一个完整的应用程序。

阅读[容器连接](http://www.widuu.com/docker/userguide/dockerlinks.html)

### 管理容器数据

现在我们知道如何连接 Docker 容器，下一步，我们学习如何管理容器数据，如何将卷挂载到我们的容器中。

阅读[管理容器数据](http://www.widuu.com/docker/userguide/dockervolumes.html)

>数据卷是一个可以供一个或多个容器使用的特殊目录。 
>
>可以达到以下目的： 
>
>1. 绕过“拷贝写”系统，以达到本地磁盘IO的性能，（比如运行一个容器，在容器中对数据卷修改内容，会直接改变宿主机上的数据卷中的内容，所以是本地磁盘IO的性能，而不是先在容器中写一份，最后还要将容器中的修改的内容拷贝出来进行同步。） 
>2. 绕过“拷贝写”系统，有些文件不需要在docker commit打包进镜像文件。 
>3. 在多个容器间共享目录 
>4. 在宿主和容器间共享目录 
>5. 在宿主和容器间共享一个文件。

### Docker Hub

现在我们应该学习更多关于使用 Docker 的知识。例如通过 Docker Hub 提供的服务来构建私有仓库。

阅读[使用Docker Hub](http://www.widuu.com/docker/userguide/dockerrepos.html)

### Docker Compose

Docker Compose 你只需要一个简单的配置文件就可以自定义你所需要的应用组件，包括容器、配置、网络链接和挂载卷。只需要一个简单的命令就可以启动和运行你的应用程序。

阅读[Docker Compose 用户指南.](http://www.widuu.com/docker/compose/README.md)

### Docker Machine

Docker Machine 可以帮助你快速的启动和运行 Docker 引擎。 Machine 可以帮助你配置本地电脑、云服务商和你的个人数据中心上的 Docker 引擎主机，并且通过配置 Docker 客户端来让它们进行安全的通信。

查阅 [Go to Docker Machine user guide.](http://www.widuu.com/docker/machine/README.md)

### Docker 集群

Docker 集群是将多个 Docker 引擎池连接在一起组合成一个独立的主机来提供给外界。它是以 Docker API 作为服务标准的，所以任何已经在Docker上工作的工具，现在都可以透明地扩展到多个主机上。

阅读 [Go to Docker Swarm user guide.](http://www.widuu.com/docker/swarm)
title: Docker的安装与初步使用
author: coolsong
tags:
  - Docker
categories: 
  - Docker
date: 2019-09-01 11:01:00
---
&nbsp;&nbsp;&nbsp;&nbsp;最近看了一些docker的文章，于是想自己试试手，就在本地安装了docker，并经行了一些初步的试水，后续的话还会学习如何部署项目到里面，以及实现自动化部署。下面主要是简单的介绍一下docker的安装，以及一些基本的概念和命令操作:
<!--more-->
### 什么是Docker
>自己最开始的理解是和虚拟机类似的东西，但是相比虚拟机有很多优势比如占用的空间更小
>相对于虚拟机的笨重，Docker则更显得轻量化，因此不会占用太多的系统资源。Docker是使用时下很火的Golang语言进行开发的，其技术核心是Linux内核的Cgroup,Namespace和AUFS类的Union FS等技术，这些技术都是Linux内核中早已存在很多年的技术，所以严格来说Docker并不是一个完全创新的技术，Docker通过这些底层的Linux技术，对Linux进程进行封装隔离，而被隔离的进程也被称为容器，完全独立于宿主机的进程。所以Docker是容器技术的一种实现，也是操作系统层面的一种虚拟化，与虚拟机通过一套硬件再安装操作系统完全不同。

容器解决了开发与生产环境的问题
>开发人员需要在本机安装各种各样的测试环境，因此开发的项目需要软件越多，依赖越多，安装的环境也就越复杂。
>同样的，运维人员需要为开发人员开发的项目提供生产环境，而运维人员除了应对软件之间的依赖，还需要考虑安装软件与硬件之间的兼容性问题。
>容器就是一个不错的解决方案，容器能成为开发与运维之间沟通的语言，因为容器就像一个集装箱一样，提供了软件运行的最小化环境，将应用与其需要的环境一起打包成为镜像，便可以在开发与运维之间沟通与传输。

>两者的区别

![My Pic](/images/docker1.png)

### 安装Docker
>由于我的电脑是window7 只能使用Docker Toolbox来安装
>安装连接主要是这两个:
>http://mirrors.aliyun.com/docker-toolbox/windows/docker-toolbox/
>https://docs.docker.com/toolbox/toolbox_install_windows/

![My Pic](/images/docker2.png)
![My Pic](/images/docker3.png)

> 安装之后会生成几个图标

![My Pic](/images/docker4.png)
>有一个需要注意的问题是 如果是用的自己的git，需要通过点击Docker Quickstart 属性 将目标中的git路径替换为自己的

### 简单使用
>点击Docker Quickstart 启动
>
![My Pic](/images/docker5.png)
>测试安装是否成功
>
![My Pic](/images/docker6.png)
>如果显示server中有报错的话 原因应该是docker machine 相关的系统环境变量没有添加成功
>运行docker-machine env default  执行上面输出的命令 添加系统变量即可

>拉取镜像并运行容器
```
# 拉取hello-world镜像 docker pull hello-world
# 使用hello-world运行一个容器 docker run hello-world
```
![My Pic](/images/docker7.png)
>显示为这样的话 就说明安装成功了

>我们可以去[DockerHub](https://hub.docker.com)找相关的镜像拉取

* 常用命令
>docker version &nbsp;&nbsp;&nbsp;打印docker版本
>docker pull name:version  拉取镜像 name：名字 version：拉取版本
>docker run name 运行一个容器
>docker images 查看所有镜像
>docker create --name name image 创建一个容器并指定名字 返回容器id
>docker container ls 查看运行中的容器 docker container ls -a 查看所有的容器
>docker ps 类似docker container ls 查看运行中的容器
>docker exec option name 进入容器
![My Pic](/images/docker8.png)
dockere image rm image_name/image_id  删除镜像
docker stop container_id 停止运行的容器
docker rm container_id 删除容器

### 在ubuntu容器中安装node
>进入容器之后 执行以下命令进行安装
```
apt-get update 
apt-get install wget 
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash 
# 安装完之后可能当前 session 读不到 nvm 命令，可以 exit 之后再进入中终端环境 
nvm install 8.0.0 
node -v
```

### 安装nginx
```
docker run -d -p 80:80 --restart=always nginx:latest
```
>参数说明： run 启动某个镜像 -d 让容器在后台运行 -p 指定端口映射，宿主机的80端口映射到容器的80端口 --restart 重启模式，设置 always，每次启动 docker 都会启动 nginx 容器
>输入docker ps  能看到启动的镜像 说明已经启动成功了
![My Pic](/images/docker9.png)

>如果本机为linux就可以直接访问localhost:80 访问nginx服务 
>但由于docker是运行在linux虚拟机上的，我们在Windows系统中运行docker，实际上是先在Windows下先安装了一个Linux环境，然后在这个环境中运行的docker。所以，访问服务中使用的localhost指的是这个Linux环境的地址，而不是我们的Windows。

* 解决办法

>首先进入nginx容器中  测试虚拟机中是否可以正常访问localhost:80端口
>![My Pic](/images/docker10.png)
>执行 curl http://localhost:80 如果报错command not find 则需要先安装 运行一下命令
>apt-get update
>apt-get install curl
>安装完成之后 再执行命令可以看到一下输出 则说明已经启动了
>![My Pic](/images/docker11.png)
>退出容器查看虚拟机地址
>docker-machine ls
>![My Pic](/images/docker12.png)
>然后就可以在浏览器中访问了
>
![My Pic](/images/docker13.png)
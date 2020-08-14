title: Docker中部署静态网页
author: coolsong
tags:
  - Docker
categories: 
  - Docker
date: 2019-09-16 00:07:00
---
&nbsp;&nbsp;&nbsp;上一篇关于docker的文章主要讲了docker的一些基本概念，以及一些经常使用的命令（创建、启动、停止）容器，以及进入容器之后的一些操作等。之后自己又尝试了在docker nginx上部署自己在github上的静态网页项目，下面就简单介绍一下具体的操作吧：
<!--more-->
### 安装Nginx

* * *


>可以先查看自己本地是否有安装过nignx
>docker image
>如果没有安装过 可以去docker hub上寻找自己想要安装的版本 通过docker pull进行安装
>通过docker create --name name创建相应镜像的容器
>创建之后 通过docker container ls -a查看所有的容器
>![My Pic](/images/docker21.png)

### 启动nginx容器

* * *

* 一般有两种启动的方式
>通过docker start id启动自己创建的nginx容器
>在使用 docker exec -it id bin/bash进入nginx容器中

>或者直接使用docker run -d -p 80:80 --restart=always nginx:latest
>参数说明： run 启动某个镜像 -d 让容器在后台运行 -p 指定端口映射，宿主机的80端口映射到容器的80端口 --restart 重启模式，设置 always，每次启动 docker 都会启动 nginx 容器。
>需要注意的是每次使用这个命令都会创建一个新的容器

>使用docker ps 查看运行中的容器
>![My Pic](/images/docker22.png)
>这样就表明容器已经启动成功
>可以访问相应的url 进行验证 这里就不贴图了

### 部署静态项目

* * *

* 进入nignx容器中
> docker exec -it id bin/bash
> ![My Pic](/images/docker23.png)

* 安装git
>使用apt-get install git
>如果报错了 可以先使用apt-get update更新一下
>安装之后 使用git version验证一下
>![My Pic](/images/docker24.png)

* 进入nginx默认网页路径中
> cd /usr/share/nginx
> 使用dir 查看当前目录的文件 会看见一个index.html

* 使用git clone url 拉取自己打包之后的项目
>![My Pic](/images/docker25.png)

* 修改nginx的配置
> 进入/etc/nginx/conf.d中 dir 可以看到一个default.conf
> 使用vim name 编辑改文件 如果报错 先使用apt-get install vim安装之后再运行命令
> 主要使用添加和修改location中映射的文件路径
```conf
server {
    listen   80;
    server_name  localhost;
    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;
    location / {
    root /usr/share/nginx/myresume/dist;
    index index.html inex.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```
* 重启nginx
> nginx -s  reload
> ![My Pic](/images/docker26.png)
> 部署成功

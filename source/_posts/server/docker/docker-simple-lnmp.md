---
title: docker搭个简单的lnmp环境
date: 2019-03-10
categories: 服务运维
tags:
	- lnmp
	- docker
	- docker-compose
---
docker 搭建lnmp有两种方式
- 一个docker容器中集成mysql php nginx
- mysql nginx 和 php 各在一个容器，多容器联合工作

<!--more-->

单容器操作简单，基于centos的docker镜像构建，其操作与在centos系统上搭建lnmp环境基本一致，适合入门学习。

## 单容器的-lnmp
单容器的lnmp通过一个 `Dockerfile` 来构建。

写Dockerfile时一般是先启动一个纯净的FROM镜像(这里是centos:7)
```
docker run --rm -it centos:7 /bin/bash
# -it 表示以交互终端的方式运行
# --rm 表示执行完就删除
```
这时候就进入镜像里了，按往常操作步骤搭建 php nginx mysql环境。并将操作步骤记录下来，转换成 `Dockerfile`中的指令。

转换时注意将步骤合理地合并，毕竟RUN指令不宜太多也不宜太少。

我这里有个写好的参考: https://github.com/talkera/docker-lnmp

单容器的缺点是复用性不强，而且格外大:这个镜像build下来大约1.9G。


## 多容器协作
多容器联合工作灵活性更强。只是容器间协作操作略复杂。

现在推荐使用docker-compose的方式，只需要配置文件即可。这个直接看`docker-compose.yml`文件就能搞明白。

这里也有写好的一个: https://github.com/talkera/docker-lnmp

不过多容器协作有几个点需要搞清楚
- nginx和php分属不同容器，但是都需要访问代码
- php要暴露fpm的9000端口供nginx访问，mysql要暴露3306端口给mysql访问

如果 `docker-compose.yml` 定义如下
```
services:
  php:
    ....
  myDatabase:
    ....
  nginx:
    ....
```
那么nginx配置如下
```
location ~ \.php$ {
	fastcgi_pass  php:9000; #php是 php容器的name
	include fastcgi_params;
	fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
	fastcgi_index index.php;
}
```

php连接数据库如下
```
$pdo = new PDO('mysql:dbname=test;host=myDatabase;port=3306','root',''); //此处的host是 mysql容器的name
```

**搭建Docker往往是辛苦一人，造福大家。**无论是`Dockerfile`还是 `docker-compose.yml` 只要有一个人写出来，其他人只需要简单看一下就将结构和依赖能了然于心，并且将环境跑起来。


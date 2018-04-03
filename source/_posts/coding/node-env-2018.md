---
title: node环境搭建
date: 2018-04-02
categories: 编程语言
tags: [javascript,node]
---

node的安装比较简单，只是需要设置一下环境变量`NODE_PATH`和`npm`的配置。否则`require`包时可能找不到。

<!--more-->

## 安装

下载地址：http://nodejs.cn/

linux解压要用到xz解压软件，可能需要单独安装。

```bash
yum install xz -y
xz -d node.tar.xz
tar -xf node.tar

#建议安装到/usr/local/node
mv node-v8.9.3 /usr/local/node
```
然后将`/usr/local/node/bin`添加到系统变量`PATH`

> node不能创建软连接到PATH包含的目录下，因为npm模块的全局安装后要在`node/bin`中创建链接

## 配置

获取`npm`的`prefix`
```bash
npm config get prefix
```
npm在全局安装时会安装到 `<prefix>/lib/node_modules`中，因此要将`NODE_PATH`设置到这个目录，这样在node控制台和js脚本中使用`require`时才能找到对应包。

另外`npm`还可以设置缓存路径

```
npm config -g set prefix /path/to/prefix
npm config -g set cache /path/to/cache
```

Linux设置环境变量 参考 {% post_link server/linux-set-path-gracefully %}


## 安装cnpm
```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
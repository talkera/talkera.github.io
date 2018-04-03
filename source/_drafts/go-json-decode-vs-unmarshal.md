---
title: Go 语言json解析使用json.Decoder 还是 json.Unmarshal
date: 2018-04-02
categories: 编程语言
tags: [go]
---

使用`json.Decoder`和`json.Unmarshal`依赖于input来自哪里。

<!--more-->

`json.Decoder`在将json字符串解析成一个Go变量之前，会将**整个JSON值缓存到内存中**。因此大多数应用场景下内存性能不会很好。

因此选择办法是：
- 如果数据来自一个io.Reader流，或者要从多个

https://stackoverflow.com/questions/21197239/decoding-json-in-golang-using-json-unmarshal-vs-json-newdecoder-decode

https://stackoverflow.com/questions/15672556/handling-json-post-request-in-go

//json trans reference:
// http://www.cnblogs.com/liang1101/p/6741262.html
// http://colobu.com/2017/06/21/json-tricks-in-Go/
// http://ethancai.github.io/2016/06/23/bad-parts-about-json-serialization-in-Golang/
// https://blog.frognew.com/2017/01/json-and-go.html
// json Unmarshal不区分大小写

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
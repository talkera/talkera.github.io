---
title: Nginx反向代理https遇上handshake报错
date: 2018-05-25
categories: 服务运维
tags:
	- nginx
	- websockt
---
报错内容大概如下：
```
SSL_do_handshake() failed (SSL: error:14077410:SSL routines:SSL23_GET_SERVER_HELLO:sslv3 alert handshake failure) while SSL handshaking to upstream
```
从日志看是ssl握手环境中，获取server hello的时候出错。

<!--more-->

## 错误原因
nginx反向代理的时候默认没有将 `server_name` 发给上游服务（被代理的服务）。
如果上游服务器配置了多个证书，这会导致上游服务器无法给出正确的证书来通信，进而导致握手失败。

开启`proxy_ssl_server_name`指令后，nginx在与上游服务进行TLS协商时，会发送`server_name`。

## 解决办法
要在virtual host做如下配置
```
location \ {
	...
	proxy_ssl_server_name on;
}
```
[指令参考在此](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_server_name)


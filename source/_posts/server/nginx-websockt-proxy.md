---
title: Nginx做websocket反向代理[译][节选]
date: 2018-04-11
categories: 服务运维
tags:
	- nginx
	- websockt
---

[原文链接](https://www.nginx.com/blog/websocket-nginx/)

Nginx从1.3版本开始支持websocket，可以为websocket的应用程序做反向代理和负载均衡。
<!--more-->
websocket协议不同于http协议，但是websocket握手和http相兼容。使用http的Upgrade功能可将http协议升级到websocket协议。这使得websocket的应用更加容易在已有设施上使用。比如使用80和443端口

websocket应用在client和server维护一个打开的持久链接。通过设置`Upgrade`和`Connection`头信息，将链接从http升级到websocket。用到的是http的upgrade机制。支持websocket的反向代理面临一些挑战。第一，`websocket`是`hop-by-hop`类型的header，因此当代理服务器拦截到client的`Upgrade`请求时，它要将自己的`Upgrade`请求发送给后端服务器，当然还需转发其他合适的请求头。第二，由于websocket链接是持久的，不像http存在时间短，反向代理服务器需要保持链接的打开状态，而不能因为链接闲置就关闭。

> http的header在有代理时根据行为分两类：端到端header(end-to-end)和逐跳header(hop-by-hop)。逐跳header只对单次转发有效，经过缓存或代理后不再转发。HTTP/1.1和之后的版本中，要使用逐跳header时需提供`Connection`字段。端到端header则会一直发送给最终接收目标。这里Nginx必须对`Upgrade`和`Connection`两个字段做转发。

Nginx通过建立一个client和server之间的通道来实现对websocket的支持。由于Nginx需要转发client到server的Upgrade请求，所以需要配置`Upgrade`和`Connection`头信息。如下：
```conf
location /wsapp/ {
    proxy_pass http://wsbackend;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
}
```
完成以上设置，Nginx就能处理websocket请求了。
---
title: SSL/TLS工作原理
date: 2017-08-11
categories: 服务运维
tags:
	- https
	- SSL/TLS
---
明文通信可能出现中间人攻击导致数据泄露或被篡改。SSL和TLS是一种保护通信的协议。

`TLS1.0`是`SSL3.0`的升级版。通常被标记为`SSL3.1`。所以通常都将两者并称为：`SSL/TLS`。以此可推`TLS 1.1`对应`SSL 3.2` etc...

两者差异并不太大，但是不能互换。[查看更多差异](https://kb.cnblogs.com/page/197396/)
目前应用最广泛的是TLS1.0和SSL3.0。

<!--more-->

## 原理
TLS协议中，客户端与服务端首先使用**非对称加密**算法通信，用于**协商**出密钥，之后用协商出的密钥将通信数据做**对称加密**，然后开始加密通信。


## 协商过程
```
客户端                                               服务端

ClientHello                  -------->
                                                ServerHello
                                               Certificate*
                                         ServerKeyExchange*
                                        CertificateRequest*
                             <--------      ServerHelloDone
Certificate*
ClientKeyExchange
CertificateVerify*
[ChangeCipherSpec]
Finished                     -------->
                                         [ChangeCipherSpec]
                             <--------             Finished
Application Data             <------->     Application Data
```
图来自[TLS RFC](https://tools.ietf.org/html/rfc5246#page-35)

1. client向server发起请求，提供自己支持的协议版本，加密算法和压缩算法 
2. server回应client，选择彼此支持的最高版本，确定加密算法和压缩算法
3. server发送自己的证书
4. client验证server的证书后，使用证书通知server变更secret。
5. client通知server开始加密通信
6. server用私钥解密获取变更后的secret 然后使用secret 将数据对称加密后发送
7. 接下来使用对称加密通信

## 一些细节
### server的证书
我们要确保server发来的证书确定是其本人，而不是中间人。
server发来证书时，证书会标识其由哪个CA签发，我们根据CA的公钥可以验证该证书是否由CA签发。
可以通过根CA验证

### 通知server变更key
常见的有两种方法：RSA和匿名Diffie-Hellman。
首先client与server在发送clientHello和serverHello中曾各生成1个随机数

RSA：
1. client验证server证书后，用证书加密一个随机数（Premaster secret），发给server
2. server使用私钥解密获得Premaster secret
3. 双方使用 Premaster secret + hello信息中的两个随机数 生成secret

Diffie-Hellman：
1. server回复serverHello后，生成一个`Server DH Parameter` 并用私钥加密后发给client
2. client使用server证书中的公钥解密获得`Server DH Parameter`
3. client将生成的`Client DH Parameter`发送给server
4. client和server使用交换的DH参数生成 secret


### https
https是http协议基于TLS的一种应用场景，其他协议也可以使用。
- FTP
- POP
- SMTP

## 参考
- [TLS RFC](https://tools.ietf.org/html/rfc5246)
- [SSL工作原理介绍以及java实现](https://blog.csdn.net/ENERGIE1314/article/details/54581411/)
- [SSL/TLS协议运行机制的概述](http://www.ruanyifeng.com/blog/2014/02/ssl_tls.html)
- [SSL与TLS的区别以及介绍](https://kb.cnblogs.com/page/197396/)
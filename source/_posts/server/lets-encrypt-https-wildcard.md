---
title: Lets Encrypted 免费Https证书 通配符/泛域名
date: 2019-11-11
categories: 服务运维
tags:
	- https
	- nginx
---

https://letsencrypt.org 支持免费的https证书。但是有效期只有三个月，因此要定时更新。

现在也支持 泛域名证书（如`*.example.com`），无需为每个域名单独签发证书。

生成证书和更新证书要使用**客户端工具**，官方推荐是 [cerbot-auto](https://certbot.eff.org/lets-encrypt/pip-other) 但是感觉  [acme.sh](https://github.com/Neilpang/acme.sh) 更好用。因此本文只介绍后者。

这两个客户端工具都是通过ACME协议与证书颁发机构通信来管理https证书。<!--more-->

> CA 即 Certificate Authority 证书签发机构
>
> ACME protocol 即 Automatic Certificate Management Environment 自动证书管理环境。是证书颁发机构和用户服务器之间的自主交互的通信协议。



## acme 的使用

https证书使用大致分如下几步

1. 获取客户端工具
2. 签发证书
3. 配置nginx的https
4. 配置自动更新

### Step1. 获取客户端工具

#### 1.直接安装

默认安装到 `/root/.acme.sh/`

```
curl https://get.acme.sh | sh
```

#### 2.使用docker
参考：https://github.com/Neilpang/acme.sh/wiki/Run-acme.sh-in-docker

```bash
docker run --rm neilpang/acme.sh
```

两种方式都是为了获取acme.sh脚本。因此直接安装的使用方式是

```bash
/path/to/acme.sh --arg1 --arg2
```

dcoker的使用方式是

```bash
docker run --rm -it -v "$(pwd)/out":/acme.sh --net=host neilpang/acme.sh \
--arg1 --arg2
```

也可以将对docker的操作封指定别名操作更方便 如

```bash
alias acme-docker='docker run --rm -it -v "$(pwd)/out":/acme.sh --net=host neilpang/acme.sh'
acme-docker --arg1 --arg2
```

### Step2. 签发证书

签发证书有多种方式，其目的是为了证明对域名的拥有权。仅介绍两种最方便的：

1. 通过添加一条 `txt` 类型的域名解析
2. 通过在网站根目录放置文件

#### 方法1.txt类型域名解析

该方法分自动（**推荐**）和手动。自动方式需要域名商提供API，配置Key和Secret即可支持**自动签发和更新**证书。

以阿里云域名解析为示例，**自动操作**如下：

```bash
#设置变量
export Ali_Key="sdfsdfsdfljlbjkljlkjsdfoiwje"
export Ali_Secret="jlsdflanljkljlfdsaklkjflsa"
#签发证书
acme.sh --issue --dns dns_ali -d example.com -d *.example.com
#复制到指定路径
acme.sh --installcert -d 'example.com'  \
--fullchain-file /data/https-cert/example.com/fullchain.cer \
--key-file /data/https-cert/example.com/example.key \
--ca-file /data/https-cert/example.com/ca.cer
```

> 其更新机制是在root账户的crontab脚本下生成一个定时任务。
>
> 所有支持的DNS服务商：https://github.com/Neilpang/acme.sh/wiki/dnsapi

**注意事项：**

- 通配符/泛域名解析，需要指定根域名和泛域名，如:`-d 'example.com' -d '*.example.com'`
- 不建议直接用生成的证书，建议生成证书后将证书拷贝到自己指定的路径。使用`--installcert`参数

**手动操作**流程如下：

1. 先执行一下签发(`issue`)操作。这一步会失败，但只是为了获取需要做解析的txt文本
2. 做`txt`类型的域名解析，需要解析的**子域名和值都会在执行上个操作后高亮显示出来**。
3. 等解析成功后，重新生成证书(`renew`)

```bash
#1.先执行签发 获取需要做解析的txt文本
/root/.acme.sh/acme.sh --issue --dns -d 'example.com' -d '*.example.com' \
--yes-I-know-dns-manual-mode-enough-go-ahead-please
#2.到阿里云做域名解析
#3.等域名解析成功后，做renew操作
/root/.acme.sh/acme.sh --renew --dns -d 'example.com' -d '*.example.com' \
--yes-I-know-dns-manual-mode-enough-go-ahead-please
#复制到指定路径
/root/.acme.sh/acme.sh --installcert -d 'example.com'  \
--fullchain-file /data/https-cert/example.com/fullchain.cer \
--key-file /data/https-cert/example.com/example.key \
--ca-file /data/https-cert/example.com/ca.cer
```

注意事项：手动签发便于测试，但**不建议用在生产环境。因为每次证书更新都必须手动重新签发(issue)  更新txt域名解析，生成(renew)**

> 手动配置的txt类型解析，在证书生成后可以手动删掉



#### 方法2.指定网站根目录（不支持泛域名）

该方法不支持泛域名，但是**支持自动更新**。其更新原理也是在 root 账户的 crontab 脚本添加定时任务。

```bash
/root/.acme.sh/acme.sh --issue -d 'example.com' -d 'www.example.com' \
--webroot /path/to/web-root/ \
--installcert \
--fullchain-file /data/https-cert/example.com/fullchain.cer \
--key-file /data/https-cert/example.com/example.key \
--ca-file /data/https-cert/example.com/ca.cer
```



### Step3. 配置nginx

```nginx
# domain自行替换成自己的域名
server {
    server_name example.com www.example.com;
    listen 443 http2 ssl;
    ssl_certificate /data/https-cert/example.com/fullchain.cer;
    ssl_certificate_key /data/https-cert/example.com/example.key;
    ssl_trusted_certificate  /data/https-cert/example.com/ca.cer; #本行可不配
}
```
如果既要https又要http
```nginx
server {
	listen 80;
	listen 443 ssl;
	#ssl on; #这一行要注释掉
	
	ssl_certificate /data/https-cert/example.com/fullchain.cer;
    ssl_certificate_key /data/https-cert/example.com/example.key;
}
```

### Step4. 更新证书

对于通过自动DNS解析和网站根目录放置文件的方式，`acme.sh`支持自动更新，**无需任何操作**。首次执行的时候其会记录相关配置，如域名、 App_key、App_Secret等。然后在root账户下生成一个定时任务，

而**手动DNS解析的方式，临近到期前需要手动重新走一遍签发、更新TXT解析、更新证书的流程**


## 参考
- 官方文档：https://github.com/Neilpang/acme.sh/wiki/How-to-issue-a-cert
- 教程：https://sb.sb/blog/linux-acme-sh-lets-encrypt-ssl/
- run in docker：https://github.com/Neilpang/acme.sh/wiki/Run-acme.sh-in-docker


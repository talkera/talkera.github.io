---
title: 记一次svn服务器迁移——Apache+SVN+HTTPS
date: 2017-06-06
categories: 工作点滴
tags: [apache, svn, https, 迁移]
---

之前Apache和svn都是源码编译安装，因为centos的yum仓库版本太低了。现在网上很多靠谱的yum源，然而版本却很新，所以用yum又快又好。

## 大概流程：

1.  在新服务器搭建svn环境，复制hooks
2.  停掉原代码库
3.  通过svnsync命令将原代码库复制过来
4.  设置新服务器的uuid
5.  将客户端代码指向新代码库

## 准备工作

首先要获取原svn服务器的uuid，如果客户端检测到svn服务器uuid跟本地不同就无法进行操作。

```
#可以在svn原服务器获取uuid
svnlook uuid /data0/repos/svn/main
#ee4fc9f8-be38-11e3-bd7e-17e849d13000

#也可以在客户端获取uuid
svn info https://svn.code.sudobash.cn/repos/main
#Repository UUID: ee4fc9f8-be38-11e3-bd7e-17e849d13000
```

## 新服务器操作

```bash
#配置svn源
#vim /etc/yum.repos.d/svn.repo
[WandiscoSVN]
name=Wandisco SVN Repo
baseurl=http://opensource.wandisco.com/centos/$releasever/svn-1.8/RPMS/$basearch/
enabled=1
gpgcheck=0

#安装相关程序
yum install svn mod_dav_svn httpd mod_ssl  openssl -y

#创建版本库
mkdir /data0/repos/svn/main
svnadmin create /data0/repos/svn/main

#svnsync会调用这个hook，如果不存在会报错。但只需要exit 0即可
touch /data0/repos/svn/main/hooks/pre_revprop_change
chmod +x pre_revprop_change
vim /data0/repos/svn/main/hooks/pre_revprop-change
#!/bin/bash
exit 0

#先绑定要同步的源 svnsync init 目标库 源库
svnsync init file:///data0/repos/svn/main https://svn.code.sudobash.cn/repos/main
#第一次同步为全量同步，耗时较长，之后为增量同步。
svnsync sync file:///data0/repos/svn/main

#修改uuid
svnadmin set uuid /data0/repos/svn/main ee4fc9f8-be38-11e3-bd7e-17e849d13000

#准备https证书
#输入common name时域名必须同svn地址一致，否则svn --trust-server-cert 会无效

mkdir /etc/httpd/ssl_ca/ && cd /etc/httpd/ssl_ca/
#创建证书
openssl genrsa 1024 > server.key
openssl req -new -key server.key > server.csr
#输入信息
Country Name (2 letter code) [XX]:CN  
State or Province Name (full name) []:Beijing
Locality Name (eg, city) [Default City]:Beijing
Organization Name (eg, company) [Default Company Ltd]:sudobash
Organizational Unit Name (eg, section) []:Dept
Common Name (eg, your name or your servers hostname) []:svn1.code.sudobash.cn
Email Address []: #空

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:    #空
An optional company name []:    #空

openssl req -x509 -days 10000 -key server.key -in server.csr > server.crt

```

### Apache配置

```
#yum安装的 mod_dav_svn 会在httpd/conf.d目录下自动生成subversion.conf，并引用dav_svn及其认证模块

LoadModule dav_svn_module     modules/mod_dav_svn.so
LoadModule authz_svn_module   modules/mod_authz_svn.so

#只需要配置虚拟主机即可
<VirtualHost *:443>
    KeepAlive On
    ServerName svn1.code.sudobash.cn
    SSLEngine on
    SSLCertificateFile "/etc/httpd/ssl_ca/server.crt"
    SSLCertificateKeyFile "/etc/httpd/ssl_ca/server.key"
    ErrorLog "logs/svn-error_log"
    CustomLog "logs/svn-access_log" common
    <Location /repos>
        DAV svn
        SVNParentPath /data0/repos/svn #配置到项目的上一层目录 可以支持多个项目
        SVNListParentPath Off   #禁止查看svn项目列表
        # AuthzSVNAccessFile /data0/repos/svn/accessControl #用于访问控制 未开启

        AuthType Basic
        AuthName "sudobash subversion"
        AuthUserFile /data0/repos/svn/authfile #svn认证，使用basic认证
        Require valid-user

        Order Deny,Allow
        Allow from 127.0.0.1 111.111.111.111 #限制访问IP
        Deny from all
    </Location>
</VirtualHost>

#需要把源服务器的basic认证文件拷贝过来
scp sudobash@org.sudobash.cn:/data0/repos/svn/authfile /data0/repos/svn/authfile

#或者用 htpasswd 重新生成一份
htpasswd -c /data0/repos/svn/authfile user1 #添加第一个用户，需创建文件，输入两遍密码
htpasswd /data0/repos/svn/authfile user2 #添加第二个用户不再需要-c


```

至此，新服务器的svn服务已经搭建完成。由于这次迁移还修改了svn地址，因此需要把客户端的代码指向新的svn仓库。

```
svn switch --relocate \
https://svn.code.sudobash.cn/repos/main \
https://svn1.code.sudobash.cn/repos/main
#relocate时使用的必须是两个仓库的根目录
```

## 客户端修改uuid

有时候会出现客户端uuid跟仓库uuid对不上的问题，需要修改客户端的uuid.

版本较高的svn，uuid保存在.svn/wc.db中，需要用sqllite3来处理。

```
# sqlite3 .svn/wc.db
sqlite> select * from REPOSITORY;
sqlite> update REPOSITORY set uuid="ee4fc9f8-be38-11e3-bd7e-17e849d13228" where id=1; 
sqlite> .exit
```

版本较低的svn uuid保存在.svn/entries中，使用sed来处理

```
sed -i 's/原uuid/新uuid/g' .svn/entries
```

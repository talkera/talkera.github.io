---
title: 优雅的设置linux环境变量
date: 2018-03-31
categories: 服务运维
tags:
	- linux
---

远程linux服务器的使用方式中，一般是普通用户登录，需要root权限的时候使用 `sudo` 或者`sudo su` 或者`sudo bash`或者`sudo su -`

然而环境变量是个麻烦的事。

<!--more-->

现在有两个需求
- `NODE_PATH`的环境变量需要为每个用户设置，并且保证su和登陆的操作都能使用
- 将node的bin目录和go的bin目录添加到PATH中

远程终端登录后的PATH以及其他一干环境变量通过执行`/etc/profile`以及家目录的 `~/.bash_profile`，此外还会执行一下家目录的`~/.bashrc`。环境变量一般都是定义在 profile 系列文件，函数和alias定义在bashrc系列文件。

这时候`sudo bash` 或`sudo su`切换到root目录后，环境变量会从 `visudo`配置的`secure_path`中获取，并且执行一下家目录的 `~/.bashrc`。家目录的`~/.bashrc`默认都包含着`/etc/bashrc`文件

包括使用su命令切换到其他用户也是一样，从`secure_path`和`~/.bashrc`中获取。

但是如果使用`su -`就会执行和终端登陆完全一样的操作。

这是因为su -是重新登录shell，而单su的操作只是shell fork一个子shell

**不管如何，反正只要是单单使用su都不会再执行`/etc/profile`了**

目前看来就只能设置到`/etc/bashrc`文件中了。

```
export PATH=$PATH:/usr/local/node-v8.9.3/bin:/usr/local/go/bin
export NODE_PATH=/usr/local/node-v8.9.3/lib/node_modules
```

但是直接在`/etc/bashrc`文件中追加设置变量会有个问题出现：从user1 sudo su再su user2后，PATH会在原来的基础上追加。
```
/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/bin:/usr/local/node-v8.9.3/bin:/usr/local/go/bin
/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/bin:/usr/local/node-v8.9.3/bin:/usr/local/go/bin:/usr/local/node-v8.9.3/bin:/usr/local/go/bin
```
NODE_PATH 因为是直接赋值，躲过一劫。

不管怎么说，这个法子都不 graceful.

按照期望：只有环境变量不符合期望时才需要修改。

简单读了一下`/etc/bashrc`文件，终于找到可趁之机。只需要在/etc/profile.d/中新建一个文件即可解决。
```
echo /etc/profile.d/set_env_path.sh <<DATA
if [ "$NODE_PATH" == "" ];then
	NODE_PATH=/usr/local/node-v8.9.3/lib/node_modules
	PATH=$PATH:/usr/local/node-v8.9.3/bin:/usr/local/go/bin
fi
DATA
```



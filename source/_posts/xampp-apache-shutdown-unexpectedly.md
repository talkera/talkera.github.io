---
title: XAMPP 无法启动Apache
date: 2018-02-17
categories: 工作点滴
tags: [apache]
---


XAMPP无法启动Apache最常见的问题就是端口被占用，然而这次问题并非端口的问题。而是因为配置文件出了错。此时启动Apache不仅不会成功，甚至连错误日志都没有。

<!--more-->
报错内容如下：
```
16:04:13  [Apache] 	Attempting to start Apache app...
16:04:14  [Apache] 	Status change detected: running
16:04:17  [Apache] 	Status change detected: stopped
16:04:17  [Apache] 	Error: Apache shutdown unexpectedly.
16:04:17  [Apache] 	This may be due to a blocked port, missing dependencies, 
16:04:17  [Apache] 	improper privileges, a crash, or a shutdown by another method.
16:04:17  [Apache] 	Press the Logs button to view error logs and check
16:04:17  [Apache] 	the Windows Event Viewer for more clues
16:04:17  [Apache] 	If you need more help, copy and post this
16:04:17  [Apache] 	entire log window on the forums

```

跟端口被占用报错一样，然而在xampp提供的netstat工具中着实没有看到80和443端口被占用的情况。于是查看Apache错误日志。居然一条日志都没有。

环境是新装的，只修改了一个vhost.conf文件。于是怀疑是vhost.conf文件的问题。清空该文件后果然成功启动了。

这个vhost.conf是之前一直在用的，按理说不可能是语法错误，只好创建一个空白的vhost.conf文件，将原来的virtual host逐个添加进行排查。这才发现是某些Apache日志的目录没有创建。

创建目录后，终于正常启动了。
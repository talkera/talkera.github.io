---
title: 记一次服务迁移
date: 2017-01-05
categories: 工作点滴
tags: [迁移]
---
之前数据库和webserver都在阿里云的杭州机房，现需要将服务器迁移到北京的阿里云。包括webServer和mysql数据库。希望迁移过程中对线上造成的影响越小越好。

迁移之前状态如下：
数据库有1个主库（master），2个从库（salve1，slave2）。其实slave2一直没有对接业务，只是作为一个备份库的存在。
网站有一个nginx，反向代理两台php-fpm。

思路如下：

1.  北京机房搭建一个nginx和两个php-fpm，但是连接杭州数据库。 通过绑定host测试北京机房网站（外网无法访问），保证程序无误。
2.  停掉slave2，并用于搭建bj-master，然后将bj-master接入到杭州机房数据库master，此时杭州机房数据库1拖3
3.  为北京机房数据库bj-master接入两个从库 bj-slave1，bj-slave2
4.  使用checksum检查北京和杭州数据库是否一致
5.  为杭州机房数据库加锁，并确认主从无延迟后，将杭州机房网站和北京机房网站链接至北京数据库bj-master
6.  修改域名解析，将服务迁移至北京机房
7.  关掉杭州服务器

本次迁移并未一天完成，前后大约2周。先将北京机房的webServer搭建了起来，链接到杭州数据库，各种测试折腾了一周。

另外这次迁移中有些内部网站使用开源系统，上传的文件保存在本地，需要专门同步到新服务器。这种静态文件还是早上CDN或者其他云存储的好。

迁移之前问过一些朋友，有建议使用 xtrabackup 对数据库进行热备。考虑到数据量确实不大（5G），而且slave2并未接入业务，所以直接从slave2 rsync了一份过去。如果以后有需要可以考虑使用。
也很庆幸这次有slave2这台备机，因为rsync的时候没考虑到带宽，直接IO打满了，还好没影响到线上。

由于业务原因，数据库里一直跑着触发器。触发器固然有其优点，但是在互联网应用里，它的存在，除了降低数据库性能外，最主要的是屏蔽了某些实现，不利于系统维护。也还好此次迁移并未有什么问题。不过还是应该找时间消灭掉触发器。
 
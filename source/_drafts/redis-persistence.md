---
title: Redis持久化[译]
date: 2018-04-11
categories: 服务运维
tags:
	- redis
---

https://blog.csdn.net/stevensxiao/article/details/51340581

http://blog.chinaunix.net/uid-20682890-id-3603246.html

http://ifelsend.com/blog/2013/03/07/%E8%A7%A3%E5%AF%86redis%E6%8C%81%E4%B9%85%E5%8C%96.html

https://www.oschina.net/question/253614_88456

https://www.cnblogs.com/shsxt/p/7911591.html

文章翻译自:https://redis.io/topics/persistence

本文提供了redis持久化的技术描述，建议所有的redis用户阅读。更多关于redis持久化和耐久性保证的信息可阅读[Redis persistence demystified](http://antirez.com/post/redis-persistence-demystified.html)

## redis 持久化
redis提供了一些持久化的选项：
- RDB持久化实现为数据集了时间点快照
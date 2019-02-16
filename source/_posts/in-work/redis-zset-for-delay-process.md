---
title: Redis的有序集合实现延时任务处理
date: 2018-11-27
categories: 工作点滴
tags: [redis,zset,php]
---

需求：文章加精后，要在半小时内，分别推送给某部分用户。时间需要分散开来。

<!--more-->

## 解决方案
假如某文章要在半小时内分3~8次推送。

可以随机生成三个时间点，然后将推送时间作为score 放到redis的有序结合中。

每分钟启动一个定时任务处理当前分钟内要推送的文章。

## 实现代码

创建文章时
```php
$newPostId = 5678;
$now = time();
$n = rand(3,8);
$keyPushPost = 'post:pushTask';
$redis = CacheRedis::instance(); //new Redis的单例

//分别生成30分钟内的 N 条记录
for($i=0; $i<$n; $i++){
	$redis->zAdd($keyPushPost, rand($now, $now+1800), "$newPostId-$i");
}
```

定时任务
```php
$redis = CacheRedis::instance(); //new Redis的单例
$start = strtotime(date('Y-m-d H:i:00'));
$end = $start + 59;

$keyPushPost = 'post:pushTask';
$data = $redis->zRangeByScore($keyPushPost, $start, $end);
foreach($data as $post){
	// ...
}
//删除处理完的任务
$redis->zRemRangeByScore($keyPushPost, 0, $end);
```
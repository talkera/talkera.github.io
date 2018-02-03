---
title: Yii项目中Memcache无法获取session
date: 2016-11-03
categories: 工作点滴
tags: [yii,memcache]
---

测试环境里有一个Yii项目突然无法从Memcache中获取到session了。

```
'cache' => [
	'class' => 'CMemCache',
	'useMemcached'=>true,
	'keyPrefix'=>'A_',
	'hashKey'=>false,
	'serializer'=>false,
	'servers'=>array(
		[
			'host'=>MC_HOST,
			'port'=>MC_PORT,
			'weight'=>60,
		],
	),
],
'session' =>[
	'class' => 'CCacheHttpSession',
	'sessionName' => 'SESS_ID',
	'timeout' => 7200,
],
```

## 排查过程

首先怀疑是Memcache服务器出了问题，但是对MC有监控，包括进程监控，读写监控，都没有报警。监控脚本是用php单独写了一组对Memcache的读写操作。所以排除了Memcache服务器的问题。

既然是程序的问题就先排查了一下session读写时，是否调用了指定的方法--即在CCacheHttpSession.php 文件中打日志。
发现session读写都是正常调用方法，调用返回值也是true。然而就是无法读取到数据，那就只能出现在对MC的操作上了。

于是判断是否因为缓存key有问题，太长或者有非法字符，导致设置失败。先从 CCache.php 文件中获取到了session在缓存中的key。然后发现：单独写php脚本对这个key进行读写操作并没有问题，而且项目中设置缓存操作的返回值也是true，所以也不是key太长或有无效字符的问题。


到这个时候已经感觉到没有可排除的地方了，只好在CMemCache.php中把接受的参数记一下看看，包括：`key value expire`。

这才找到原因。因为缓存的过期时间，是 time()+$expire，而测试服务器的时间被修改了，跟MC所在服务器时间相差了4个小时，导致缓存在设置的时候，已经过了超时时间变为无效了。

## MC的缓存有效期设置

php在为memcache设置有效期时，可以直接设置有效时长，也可以设置有效截止时间。但这两种设置方法是靠对参数值的判断来实现的。设置有效时长最多只支持到30天（30*24*3600）。如果大于30天就要设置有效截止日time()+expire。

Yii中默认的是设置有效截止时间，因此出现了这样的问题

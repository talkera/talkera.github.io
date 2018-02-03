---
title: PHP反序列化session
date: 2017-08-06
categories: 编程语言
tags: [PHP,session]
---
一般情况下php的session都是序列化成字符串保存的，通过$_SESSION可以直接操作session。

最近有个需求要读取保存在memcache中session字符串。网上找了一圈后发现，PHP似乎没有提供直接反序列化session的函数，只好自己写了一个。

PHP中的session序列化并不是单纯的serialize，而是形如：

```
key1|serialize后的value1;key2|serialize后的value2
```


因此解析的关键在于使用 '|' 和 ';' 分割数据。

```
function parseSession($str){
	$sessionData = [];
	while(strpos($str, '|')){
		$p = strrpos($str, '|');
		$v = substr($str, $p+1);
		$str = substr($str, 0, $p);

		$p = strrpos($str, ';');
		if($p){
			$k = substr($str, $p+1);
			$str = substr($str, 0, $p);
		}else{
			$k = $str;
			$str = '';
		}
		$sessionData[$k] = unserialize($v);
	}
	return $sessionData;
}
```


 

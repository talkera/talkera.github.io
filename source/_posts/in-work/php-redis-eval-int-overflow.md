---
title: PHP的redis库执行lua脚本结果溢出
date: 2020-12-14
categories: 工作点滴
tags: [redis,php,lua]
---

安装的PHP是32位的，导致处理int型返回值溢出。

<!--more-->

## 问题
有一段这样的代码，执行结果是：`int(2147483647)`
```php
$lua = <<<LUA
local a = 12345678901
return a
LUA;
return $redis->eval($lua);
```

显然是溢出导致的。

第一步怀疑redis的处理有问题，虽然感觉不太可能。

于是用wireshark抓包看redis的返回值，发现**返回值正常** `:12345678901` 。

```
*3
$4
EVAL
$31
local a = 12345678901
return a
$1
0
:12345678901
```

也不太可能是PHP的redis扩展有问题，毕竟都在用的东西，要是有bug早就炸了。

最后不知怎么想到了PHP的版本，发现是32位的。于是找了一台64位的PHP，发现果然没有溢出的问题了。
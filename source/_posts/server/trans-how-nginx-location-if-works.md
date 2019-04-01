---
title: Nginx中的if如何工作的[译]
date: 2019-04-01
categories: 服务运维
tags:
	- nginx
	- rewrite
---

[原文链接](http://agentzh.blogspot.com/2011/03/how-nginx-location-if-works.html)

人言“if 很邪门”。其实搞懂了就不邪门了。

<!--more-->

## 翻译正文之前的准备
nginx处理请求分多个阶段(phase) 并非顺序执行。 如
- 第2个阶段：rewrite阶段 NGX_HTTP_REWRITE_PHASE
- 第10个阶段：内容产生阶段 NGX_HTTP_CONTENT_PHASE

指令的执行顺序和配置关系不大，而是由模块实现决定的。

每个阶段包含若干个 handler，处理到某个阶段的时候，依次调用该阶段的 handler 处理请求。

如在内容产生阶段，为了给一个request产生正确的响应，nginx必须把这个request交给一个合适的content handler去处理。

## 翻译开始
nginx 的 if 指令确实在实践中会有奇怪的表现。
人们如果对其了解不够，可能会误用这个指令。
这篇文章里，作者分析了一些示例，以帮助人们正取使用。

简单来说，nginx内 if 指令**创建了一个内嵌的 location 块**。
一旦 if 的条件匹配，那么只有里面的 content handler 被执行。


示例 1
```
location /proxy {
	set $a 32;
	if ($a = 32) {
		set $a 56;
	}
	set $a 76;
	proxy_pass http://127.0.0.1:$server_port/$a;
}

location ~ /(\d+) {
	echo $1;
}
```
上述配置中 如果请求 `/proxy` 会显示76。其执行步骤如下：

1. nginx按配置文件中的顺序，**仅执行**所有的 rewrite 阶段的指令。
```
set $a 32;
if ($a = 32) {
	set $a 56;
}
set $a 76;
```
此时 $a 获取到了最终值 76

2. nginx 陷入 if 块，因为 $a == 32 的条件在第1步的时候满足了
3. 此时 if 的内部块没有包含任何 `content handler`，那么 ngx_proxy 会继 承外部块的 content handler（外部的ngx_proxy）
4. proxy指定的配置也被内部的if继承
5. 请求终止（控制流永远走不出 if 块）

也就是说，在这个示例中，外部块的 proxy_pass 指令永远都不会执行。其实是 if 块内继承的在服务。

下面看一下，当 if 块内复写 content handler 后会发生什么。

示例2
```
location /proxy {
	set $a 32;
	if ($a = 32) {
		set $a 56;
		echo "a = $a";
	}
	set $a 76;
	proxy_pass http://127.0.0.1:$server_port/$a;
}

location ~ /(\d+) {
	echo $1;
}
```
访问 /proxy 会得到 a = 76

看起来与预期不符？看一下这次发生了什么：

1. nginx按配置文件中的顺序，**仅执行**所有的 rewrite 阶段的指令。
```
set $a 32;
if ($a = 32) {
	set $a 56;
}
set $a 76;
```
$a 的最终值是76

2. nginx 陷入 if 块，因为 $a == 32 的条件在第1步的时候满足了
3. 内部块有了 content handler "echo"，然后 $a 的值 76 就被输出到客户端了
4. 请求终止。控制流还是没有走出if块，同示例1一样

我们确实有办法让示例2 按我们期望的工作：

示例3
```
location /proxy {
set $a 32;
if ($a = 32) {
	set $a 56;
	break;

	echo "a = $a";
}
set $a 76;
	proxy_pass http://127.0.0.1:$server_port/$a;
}

location ~ /(\d+) {
	echo $1;
}
```
这次我们在 if 的块添加了一个 break 指令。该指令会让nginx停止执行剩下的ngx_rewrite指令。所以这次结果是：
```
a = 56
```
这次 nginx 工作方式如下：
1. nginx按配置文件中的顺序，**仅执行**所有的 rewrite 阶段的指令。
```
set $a 32;
if ($a = 32) {
	set $a 56;
	break;
}
```
$a 的最终值是56

2. nginx 陷入 if 块，因为 $a == 32 的条件在第1步的时候满足了
3. 内部块有了 content handler "echo"，然后 $a 的值 56 就被输出到客户端了
4. 请求终止。控制流还是没有走出if块，同示例1一样

现在可以看出，关键原因在于：在嵌套的locations中，ngx_proxy的如何继承配置。
但其他模块在嵌套的location中，可能不会继承外部块。如 echo 模块。事实上，大多数 content handler 包括 upstream 都不会继承。

此外在 if 块的配置继承上，还有必须注意的副作用，考虑下面的例子：
```
location /proxy {
  set $a 32;
  if ($a = 32) {
	  return 404;
  }
  set $a 76;
  proxy_pass http://127.0.0.1:$server_port/$a;
  more_set_headers "X-Foo: $a";
}

location ~ /(\d+) {
  echo $1;
}
```
此处 `ngx_header_more`的 `more_set_headers` 同样会被 if 块创建的 location 块继承。所以会得到
```
HTTP/1.1 404 Not Found
Server: nginx/0.8.54 (without pool)
Date: Mon, 14 Feb 2011 05:24:00 GMT
Content-Type: text/html
Content-Length: 184
Connection: keep-alive
X-Foo: 32
```
这应该不是期望中的

顺便说一句，在这个示例中，如果换成 add_header 指令将不会输出 X-Foo 头。但这不能说明这里没有发送指令继承，而是因为add_header的 header filter 会跳过404响应。

可见if指令背后的陷阱之多。难怪人们一直说 “if很邪门”

我们已经用 ngx_lua 模块，通过lua语言来处理这样复杂分支的nginx配置（以及整个应用的业务逻辑）。Lua的 if 一点也不邪门。

使用ngx_lua的set_by_lua指令，甚至不会有Lua协程的额外开销(尽管开销非常小)。

请注意我不是说绝对不要使用nginx的if，请不要误解。我写这篇文章的动机只是想解释隐藏在底层的机制并帮助你正确的使用它

我认为Igor Sysoev在开发2.0的nginx 时会重新设计整个rewrite模块，届时一切都会改变的。
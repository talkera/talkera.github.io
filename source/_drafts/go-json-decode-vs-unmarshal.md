---
title: Go 语言json解析使用json.Decoder 还是 json.Unmarshal
date: 2018-04-02
categories: 编程语言
tags: [go]
---

使用`json.Decoder`和`json.Unmarshal`依赖于input来自哪里。

<!--more-->

`json.Decoder`在将json字符串解析成一个Go变量之前，会将**整个JSON值缓存到内存中**。因此大多数应用场景下内存性能不会很好。

因此选择办法是：
- 如果数据来自一个io.Reader流，或者要从多个

https://stackoverflow.com/questions/21197239/decoding-json-in-golang-using-json-unmarshal-vs-json-newdecoder-decode

https://stackoverflow.com/questions/15672556/handling-json-post-request-in-go

//json trans reference:
// http://www.cnblogs.com/liang1101/p/6741262.html
// http://colobu.com/2017/06/21/json-tricks-in-Go/
// http://ethancai.github.io/2016/06/23/bad-parts-about-json-serialization-in-Golang/
// https://blog.frognew.com/2017/01/json-and-go.html
// json Unmarshal不区分大小写

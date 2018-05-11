---
title: Go实现工厂模式的思考
date: 2018-05-11
categories: 编程语言
tags: [go, 设计模式]
---
模式从业务场景中总结出来，然而并不是固定的。

<!-- more -->

## 起因
有一个多平台的抓取需求，必然要提炼一个方法，根据平台类型获取实例，每个实例有各自的实现。这是个典型工厂模式。

经典OOP伪代码如下：
```
class siteModel{
	url string
	func factory(siteName, url string){
		if siteName == "site1"{
			return new site1(url)
		}
	}
}
class site1 extends siteModel{
	func construct(url string){
		this.url = url
	}
	func fetch(){}
}
model := siteModel::factory("site1")
model.fetch()
```

然而**golang是没有类似java那样的继承和重载的。**透过表面看本质，go提供了更直接，更简单的实现。

**所谓工厂只是定义了一些必须实现的方法**，正好对应了go的interface 只是go并不做强制要求。

```go
type site interface {
	fetch()
}

type siteModel struct {
	URL string
}
type site1 struct {
	siteModel
}

func (s site1) fetch() {
	fmt.Println("site1 fetch data")
}

func factory(s string) site {
	if s == "site" {
		return site1{
			siteModel{URL: "http://www.xxxx.com"},
		}
	}
	return nil
}

func main() {
	s := factory("site")
	s.fetch()
}
```

设计模式是前辈从业务场景中抽象出来的实现方式，在不同语言中有不同的实现方式。
go中虽然看起来没有面向对象的特性，但其实是把这些特性以春风化雨般的方式融入到了语言本身，剥离了复杂的外表，让设计模式的使用更加灵活。
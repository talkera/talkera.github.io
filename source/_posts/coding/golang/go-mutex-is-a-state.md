---
title: go的互斥锁只是一个君子协定
date: 2018-04-13
categories: 编程语言
tags: [go, race condition]
---

2018-11-22更新
这是我初学go的一篇笔记，在此之前并无并发相关的开发经验。
当时很好奇go的互斥锁如何生效。经过这个实验我了解到：对于这种可能造成竞态的代码，正常流程应该是 `获取锁 -> 操作 -> 释放锁`。需要主动获取锁，这样锁才能生效。
而在mysql中，如果某条记录被加锁（比如select for update），其他写操作不需要主动获取锁，就会自动被阻塞无法进行。因为mysql中update **获取锁**的操作是**自动的，对用户透明**。所以我之前一直误以为`一个锁应当能保护变量或内存或代码段`，才有了这个笔记。

这个笔记想表达的是，锁的正确用法应当是`获取锁 -> 操作 -> 释放锁`，开发需遵守这个约定，否则锁不会生效。仅此而已。

谢谢简书评论区各位指正。修改后正文如下：

golang中的互斥锁并不能锁定任何内存或代码或变量。

<!--more-->
下面代码先启动一个goroutin将变量锁住，然后在main函数里直接操作。
```go
var wg sync.WaitGroup
var name = "hello world"
var mu sync.Mutex

func main() {
	wg.Add(1)
	go modify()

	<-time.After(1 * time.Second) //等待一秒钟保证go routine执行
	name = "dddddd" //有锁应该修改失败或者阻塞
	fmt.Println("in main", name)

	wg.Wait()
}

func modify() {
	defer wg.Done()
	mu.Lock()
	defer mu.Unlock()

	name = "modify"
	fmt.Println("in modify", name)
	<-time.After(2 * time.Second)
	fmt.Println("in modify", name)
}
```
结果
```
in modify modify
in main dddddd
in modify dddddd
```
结果表示modify中确实修改了变量，但是在modify加锁时，main函数仍然成功修改了变量，意味着`Lock`并不能保护任何内存或变量，不在乎锁的代码仍然可以随便操作。

go的mutex只是一个君子协定：**在有可能造成race condition的地方，通过尝试获取锁`mu.Lock()` 来决定能否操作。而非强制性保护**。

所以必须在所有可能造成race condition的代码处加上`Lock()`，否则这段代码会无视锁的存在，可以随意操作了。

mian函数中正确的用法应该是先`获取锁`
```go
func main() {
	wg.Add(1)
	go modify()

	<-time.After(1 * time.Second) //等待一秒钟保证go routine执行

	mu.Lock() //先获取锁
	name = "dddddd" //有锁应该修改失败或者阻塞
	fmt.Println("in main", name)
	mu.Unlock()

	wg.Wait()
}
```
输出结果
```
in modify modify
in modify modify
in main dddddd
```
结果说明main在获取锁的时候被阻塞，等modify执行完后，才获取到锁，才修改了变量。

## 延伸
`mutex`变量可以定义多个，用来保护不同操作对象的代码段。然而需要保护同一个对象的代码必须使用同一个`mutex`才会生效。所以最好按需定义，如：
```go
type data struct{
	muIn sync.Mutex //用于保护data内的数据同一时间只能有一处操作
	v int
}
var muOut sync.Mutex //用于保护其它操作对象
```

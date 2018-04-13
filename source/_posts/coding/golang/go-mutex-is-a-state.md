---
title: go的互斥锁只是一个君子协定
date: 2018-04-13
categories: 编程语言
tags: [go, race condition]
---
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

	name = "dddddd" //有锁应该修改失败或者阻塞
	fmt.Println("in main", name)

	wg.Wait()
}

func modify() {
	defer wg.Done()
	mu.Lock()
	defer mu.Unlock()

	name = "modify"
	<-time.After(2 * time.Second)
	fmt.Println("in modify", name)
}
```
结果
```
in main dddddd
in modify dddddd
```
main函数里成功修改，意味着`Lock`并不能保护任何内存或变量，不使用锁的代码仍然可以随便操作。

go的mutex只是一个君子协定：**在有可能造成race condition的地方，通过判断状态（是否已Lock）来决定能否操作。而非强制性保护**。即：`Lock()`之前先通过状态判断是否为“可以操作”，如果可以就修改状态，保证其他goroutine查询状态的结果是“不可操作，需等待”。修改完成后将状态改为“可以操作”，后续gouroutine才能接手。

所以必须在所有可能造成race condition的代码处加上`Lock`，保证其遵守君子约定，否则这段代码就会是一个“野蛮人”可以随意操作了。

`mutex`变量可以定义多个，用来保护不同操作对象的代码段。然而需要保护同一个对象的代码必须使用同一个`mutex`才会生效。所以最好按需定义，如：
```go
type data struct{
	muIn sync.Mutex //用于保护data内的数据同一时间只能有一处操作
	v int
}
var muOut sync.Mutex //用于保护其它操作对象
```

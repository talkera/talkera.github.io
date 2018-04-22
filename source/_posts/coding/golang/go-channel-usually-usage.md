---
title: go channel的常规用法
date: 2018-04-21
categories: 编程语言
tags: [go, channel]
---
整理的channel常规用法

<!--more-->

## 循环获取channel
```go
func get(data chan int){
    for v,ok := range chan{
        if !ok{
            //channel 已经关闭
            break
        }
        // do something with v
    }
}
```
如果需要停止使用channel，需要手动将channel关闭
```go
close(data)
```
关闭后的channel还能获取其中存在的数据，但是不能再增加数据。数据取完后`ok`值为false。

## channel关闭的判断
```go
ch = make(chan int, 10)
//....some code
select{
    case r,ok := <- ch:
    if !ok  {
        //通道已空 并且已经关闭
    }
}

```

## 向有缓存的channel传数据，满了就停止，不阻塞

```go
ch = make(chan int, 10)

Fill: //为循环设置tag
for{
    select {
        case ch <- 1:
        default:
            break Fill
    }
}
```
for循环必须设置tag，不然select中的break无法停止外部循环，会一直执行default，陷入死循环。
```go
//这段代码会陷入死循环中，每次都执行default
for{
    select{
    case <- time.After(10*time.Second):
    default:
        break
    }
}
```

## 超时的使用
```go
select{
    case job <- jobList
    case <- time.After(10 * time.Second):
    //10秒后做超时处理
}
```
---
title: Go面试题--2018
date: 2018-04-09
categories: 编程语言
tags: [go]
---

近日公司面试招聘golang开发，面试中有

<!--more-->

```go
package main

import (
	"errors"
	"fmt"
	"sync"
	"sync/atomic"
	"time"
)

//节流器

type throttle struct {
	D      time.Duration //周期是D，
	C      int64         //限制一个周期最多操作C次
	Mu     sync.Mutex
	Token  chan bool //令牌池
	num    int64     //当前的goroutine数量
	maxNum int64     //允许工作goroutine最大数量
}

//如果两个周期后还没有申请到令牌，就报错超时
//目前用不到，如果限制routine最大数量需要靠这来监控
var ErrApplyTimeout = errors.New("apply token time out")

func NewThrottle(D time.Duration, C, maxNum int64) *throttle {
	instance := &throttle{
		D:      D,
		C:      C,
		Token:  make(chan bool, C),
		maxNum: maxNum,
	}
	go instance.reset()
	return instance
}

//每周期重新填充一次令牌池
func (t *throttle) reset() {
	timer := time.NewTimer(t.D)
	for {
		<-timer.C //阻塞1个周期
		//goroutine数量不超过最大数量时再填充令牌池
		if t.num < t.maxNum {
			t.Mu.Lock()
			supply := t.C - int64(len(t.Token))
			fmt.Printf("reset token:%d\n", supply)
			for supply > 0 {
				t.Token <- true
				supply--
			}
			t.Mu.Unlock()
		}
		timer.Reset(t.D) //重置定时器
	}
}

//申请令牌，如果过两个周期还没申请到就报超时退出
func (t *throttle) ApplyToken() (bool, error) {
	select {
	case <-t.Token:
		return true, nil
	case <-time.After(t.D * 2):
		return false, ErrApplyTimeout
	}
}

func (t *throttle) Work(job func()) {
	if ok, err := t.ApplyToken(); !ok {
		fmt.Println(err)
		return
	}
	go func() {
		atomic.AddInt64(&t.num, 1)
		defer atomic.AddInt64(&t.num, -1)
		job()
	}()
}
func main() {
	t := NewThrottle(time.Second, 10, 20) //每秒10次，同时最多20个routine存在
	for {
		t.Work(doWork)
	}
}

//真正的工作函数 假设每个需要执行5秒
func doWork() {
	fmt.Println(time.Now())
	<-time.After(5 * time.Second)
}
```
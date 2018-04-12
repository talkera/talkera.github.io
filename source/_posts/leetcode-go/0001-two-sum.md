---
title: 0001-两数之和
date: 2018-04-11
categories: leetcode-go
tags: [go, leetcode]
---
> 给定一个整数数列，找出其中和为特定值的那两个数。

<!--more-->

你可以假设每个输入都只会有一种答案，**同样的元素不能被重用。**

示例:
```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```


```go
package main

import "fmt"

func main() {
	fmt.Println(twoSum([]int{2, 7, 11, 15}, 9))
}

func twoSum(nums []int, target int) []int {
	num2 := make(map[int]int, len(nums))
	for k,v := range nums{
		needV := target - v
		if index,ok := num2[needV]; ok{
			return []int{index, k}
		}
		num2[v] = k
	}
	return nil //不能省略
}
```
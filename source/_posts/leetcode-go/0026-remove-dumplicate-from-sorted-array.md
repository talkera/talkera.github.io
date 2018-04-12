---
title: 0026-从排序数组中删除重复项
date: 2018-04-08
categories: leetcode-go
tags: [go]
---
> 给定一个有序数组，你需要原地删除其中的重复内容，使每个元素只出现一次,并返回新的长度。不要另外定义一个数组，您必须通过用 O(1) 额外内存原地修改输入的数组来做到这一点。

<!--more-->

示例：
```
给定数组: nums = [1,1,2],

你的函数应该返回新长度 2, 并且原数组nums的前两个元素必须是1和2
不需要理会新的数组长度后面的元素
```

不重复的数据为有效数据，放在前面，用一个值来记录有效数据的长度。

遍历数组的过程中遇到重复的就跳过。遇到不重复的，并且和最后一个有效数据不相连的，就移动到最后一个有效数据后面。
```go
package main

func main()  {
	data := []int{1,1,2}
	println(removeDuplicates(data))
}

func removeDuplicates(nums []int) int {
	if len(nums)<=1{
		return len(nums)
	}

	res := 1
	i :=1
	for ; i<len(nums); i++{
		if nums[i] == nums[i-1]{
			continue
		}
		if res != i {
			nums[res] = nums[i]
		}
		res++
	}
	return res
}

```
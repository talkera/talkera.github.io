---
title: Golang 重建二叉树
date: 2019-03-20
categories: 编程语言
tags:
  - go
  - 算法
  - 二叉树
---
这是剑指offer的一道题。

> 输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建出如下图所示的二叉树并输出它的头结点。　

<!-- more -->

思路如下：
- 中序遍历中根节点之前的节点属于左子树，之后为右子树。
- 前序遍历的第一个节点为根节点。
- 前序遍历结果的排列为：根节点-左子树节点-右子树节点
- 拆分出左子树和右子树递归重复上述过程

```
package main

import (
	"fmt"
)

// 重建二叉树

type Node struct {
	value int64
	left *Node
	right *Node
}

func main() {
	preOrder := []int64{1,2,4,7,3,5,6,8}
	inOrder := []int64{4,7,2,1,5,3,8,6}

	tree := constructBTree(preOrder, inOrder)
	
	//将构建好的二叉树 输出先序遍历和中序遍历的结果 用于检验
	preCatTree(tree)
	inCatTree(tree)
}

//重建二叉树
func constructBTree(preOrder, inOrder []int64) *Node{
	l := len(preOrder)
	if l == 0{
		return nil
	}

	root := &Node{
		value:preOrder[0],
	}
	if l == 1{
		return root
	}

	leftLen := 0
	rightLen := 0
	for _,v := range inOrder{
		if v == root.value{
			break
		}
		leftLen++ //根节点之前的为左子树长度
	}
	rightLen = l - leftLen - 1 //右子树长度

	if leftLen > 0{
		//fmt.Println("左子树",preOrder[1:leftLen+1], inOrder[0:leftLen]) //可打开注释查看详细过程
		root.left = constructBTree(preOrder[1:leftLen+1], inOrder[0:leftLen])
	}
	if rightLen >0{
		//fmt.Println("右子树",preOrder[leftLen+1:], inOrder[leftLen+1:])
		root.right = constructBTree(preOrder[leftLen+1:], inOrder[leftLen+1:])
	}

	return root
}


func preCatTree(t *Node) {
	fmt.Println(t.value)
	if t.left!=nil{
		preCatTree(t.left)
	}
	if t.right!=nil{
		preCatTree(t.right)
	}
}
func inCatTree(t *Node) {
	if t.left!=nil{
		inCatTree(t.left)
	}
	fmt.Println(t.value)
	if t.right!=nil{
		inCatTree(t.right)
	}
}

```
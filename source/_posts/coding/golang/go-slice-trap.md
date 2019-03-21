---
title: Golang slice的坑
date: 2019-03-20
categories: 编程语言
tags: 
  - go
  - slice
---

slice的底层是数组，其内部包含三个属性：分别是：ptr, len, cap
- ptr 是指向底层数组的指针
- cap 是底层数组的长度
- len 是slice的长度

<!-- more -->

```
type slice struct {
    array unsafe.Pointer
    len   int
    cap   int
}
```

当slice长度不足以放下新元素时，会将当前的数据复制到一个更大的数组中。自动扩容时，长度在1024以下的每次扩容到原数组的2倍。大于1024的每次扩容1.25倍。

这种特性会导致很多特殊情况。

### slice是否被修改
```
package main
import "fmt"
func main(){
	s := make([]int, 3, 3) //[0 0 0]
	modifyA(s)
	fmt.Println(s)

	modifyB(s)
	fmt.Println(s)

	modifyB(s[0:2])
	fmt.Println(s)

	modifyC(s[0:2:2])
	fmt.Println(s)

	w := s[1:]
	w[0] = 6
	fmt.Println(w)
	fmt.Println(s)
}

func modifyA(x []int){
	x[1] = 1
}
func modifyB(x []int){
	x = append(x, 2)
}
func modifyC(x []int){
	x = append(x, 4)
}
func modifyD(x []int){
	x[0] = 3
}
```

调用modifyA会修改。因为将 `ptr`传递给了函数 所以操作的底层数组还是同一个。
```
modifyA(s)
fmt.Println(s) //被修改了 [0 1 0]
```

第一次调用 modifyB没有修改。因为go函数调用为值传递。
操作的底层数组由于是复制指针，所以指向同一个数组。但`len`和`cap`则是复制来的。
函数中，是在`len`之后添加元素，只修改了局部变量`x` 的`len`，却不会影响原本的`s`。
如果在modifyB函数中输入 `x[3]` 则能输出值。
```
modifyB(s)
fmt.Println(s) //没有被修改 [0 1 0]
```

第二次调用 modifyB 则修改了。
因为 `s[0:2]` 的 `len` 是2，再追加其实是给`x[2]`赋值。所以直接修改了底层数组，影响了`s`
```
modifyB(s[0:2])
fmt.Println(s) //被修改了 [0 1 2]
```

modifyC虽然与modifyB类似，但是调用时传递的`s[0:2:2]` `cap` 为2，`len`也为2，此时空间已经不足以append，所以做了扩容，扩容后 ptr 会变。此时`x`与`s`指向的已经不是同一个底层数组了。所以不会修改
```
modifyC(s[0:2:2])
fmt.Println(s) //没有被修改 [0 1 2]
```
此处 `s[0:2:2]` 最后一个冒号后面的数字，明确声明了cap的值为2，所以才能不修改原数组。平时应用时，除非用`cap(s) == len(s) `来显示确认，否则无法确定 `s`是否会被修改。

其实核心内容有两点：
- 函数为值复制传递
- slice的属性包含指向底层数组的指针、cap和len

据此就能对上面的情况就能做出正确判断了。

### copy的坑
如果要做slice复制，需要使用`copy(dst, src)`函数。

copy复制的元素个数其实是`min(len(dst), len(src))`。如果dst是一个`len=0`的slice是无法复制进去的。

真正的用法应当是这样
```
arr := []int{1, 2, 3}
tmp := make([]int, len(arr))
copy(tmp, arr)
```

---
title: Go类型转换和断言总结
date: 2018-04-25
categories: 编程语言
tags: [go]
---
Go类型转换和断言总结

<!--more-->

## 普通类型转换
将类型名作为函数名，被转换的值作为参数。使用于跨度小的转换。
```go
//[]byte和string的转换
b := []byte{'a','b','c'}
str := string(b)
b = []byte(str)

//int int64 int32的转换
var x int = 0x123456789
y := int32(a)
z := int64(a)
x = int(y)
```

## string和int
int => string
```go
//int => string
d := 12345
s := strconv.Itoa(d)

//int64 => string
var d64 int64 = 12345
s := strconv.FormatInt(d64, 10) //第二个参数是进制

//没有int32 转 string的，不过可以先把int32转成int64
var d32 int32 = 12345
s = strconv.FormatInt(int64(d32), 10)
```
string => int
```go
//string => int
s := "12345"
d := strconv.Atoi(s)

//string => int64
d64,err := strconv.ParseInt(s, 10, 64)
//参数2是进制，参数1是必须满足的int类型：0对应int 8对应int8 32对应int32依次类推
//返回值是int64
```

`strconv.ParseInt("123", 16, 8)` 会报错，因为要求满足int8，所以16进制的数最多只能给2位

## string和float
float64转字符串
```go
var f64 float64 = 1232323232.3456323232
strconv.FormatFloat(f64, 'f', 3, 64)
```
参数含义分别为：格式，保留小数位数，满足的类型（32或64）

第二个参数（格式）限定为：b e E f g G 中的一个 分别对应不同的格式。格式和结果对应如下：
- `b` 5168738262720215p-22
- `e` 1.232e+09
- `E` 1.232E+09
- `f` 1232323232.346 有四舍五入
- `g` 1.23e+09
- `G` 1.23E+09

string转float
```go
s := "12345.6789"
strconv.ParseFloat(s, 64)
//第二个参数只要满足的类型float32 或 float64
```

## interface的处理
以面向对象的思维来看，几乎所有的类型都可以理解成interface的子类。然而要调用子类方法时必须将interface转换为特定子类。

interface的类型转换方式较为特殊
```go
var a interface{} = "xxxxx"
s := a.(string) //转换成字符串
```

直接断言，如果失败会报panic错误
```go
var a interface{} = "xxxxx"
s := a.(int)//会报panic错误
```

因此断言前可以添加判断
```go
var i interface{} = "ssss"
str, ok := i.(int)
if !ok {
	fmt.Println("is not int")
} else {
	fmt.Println(str)
}
```

### 与switch结合
```go
var i interface{} = "sssss"
switch i.(type){
	case "int":
		fmt.Println("it's int")
	case "string":
		fmt.Println("it's string")
	default:
		fmt.Println("unknown type")
	}
}
```
`变量.(type)`可以直接反射出变量的类型。并且只能和switch结合使用。

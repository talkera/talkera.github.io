---
title: Golang解析json的特殊情况处理
date: 2018-05-11
categories: 编程语言
tags: [go]
---
Go解析json遇到了大数字、不定格式等特殊情况，在此做了一个整理。
<!-- more -->
## Unmarshal vs Decode
选择哪个要视输入而定。 

`json.Unmarshal` 操作对象是一个 `[]byte`，也就意味着被处理的JSON要全部加载到内存。如果有一个加载完的JSON使用`json.Unmarshal`会快一些。

`json.Decoder` 操作的是一个`stream`，或者其他实现了`io.Reader`接口的类型。意味着可以在接收或传输的同时对其进行解析。当处理一组较大数据时无需重新copy整个JSON到内存中。

最好的选择办法如下：
- 如果数据来自一个`io.Reader`或者需要从一个`stream`中读取数据，就选择`json.Decoder`
- 如果已经将整个JSON加载到内存中了就使用`json.Unmarshal`

## 数字的解析
默认情况下，go对json解析过程中遇到的数字都会当做float64处理。如果数字过大会有精度丢失。可以使用json.Number来处理。

### Unmarshal
```go
val := `{"id": 100010001000100010001000 }` //26位数字
var y map[string]json.Number
json.Unmarshal([]byte(val), &y)
fmt.Println(y) //map[id:100010001000100010001000]

z, _ := json.Marshal(struct {
	Id json.Number `json:"id"`
}{y["id"]})
fmt.Println(string(z)) //{"id":100010001000100010001000}
```

### Decode
```go
val := `{"id": 100010001000100010001000 }` //26位数字
val2 := strings.NewReader(val)             //先转成io.Reader
d := json.NewDecoder(val2)
d.UseNumber() //标记使用josn.Number

var x map[string]interface{}
if err := d.Decode(&x); err != nil {
	panic(err)
}
fmt.Printf("%#v\n", x) //相应值的Go语法表示

newJson, _ := json.Marshal(x)
fmt.Println(string(newJson)) //json.Number编组结果
```
输出结果：
```
map[string]interface {}{"id":"100010001000100010001000"}
{"id":100010001000100010001000}
```
使用`json.Decoder`只能操作`io.Reader`类型的JSON数据。


## 不定类型的解析
有时候遇到字段不定的JSON，需要一边判断一边解析。如：
```go
t1 := `{"type":"a", id:"aaa"}`
t2 := `{"type":"b", id:22222}`
```
### 解组到interface{}
可以先统一解组到interface{} 然后判断关键字段再进行后续处理。
```go
type Data struct {
	Type string      `json:"type"`
	Id   interface{} `json:"id"`
}

func decode(t string) {
	var x Data
	err := json.Unmarshal([]byte(t), &x)
	if err != nil {
		panic(err)
	}
	if x.Type == "a" {
		fmt.Println(x.Id.(string))
	} else {
		fmt.Println(x.Id.(float64)) //json解析中number默认作为float64解析
	}
}
func main() {
	t1 := `{"type":"a", "id":"aaa"}`
	t2 := `{"type":"b", "id":22222}`

	decode(t1)
	decode(t2)
}
```
结果
```
aaa
22222
```

### 使用`json.RawMessage`
使用RawMessage便于分步Unmarshal

```go
type Resp struct {
	Type string          `json:"type"`
	Data json.RawMessage `json:"data"`
}
type Data struct {
	Id json.Number `json:"id"` //处理大数
}

func main() {
	t := `{"type": "a", "data":{"id": 1234567890123456789012345}}`

	var x Resp
	var y Data

	json.Unmarshal([]byte(t), &x)

	//进一步解组
	if "a" == x.Type {
		json.Unmarshal(x.Data, &y)
	}

	fmt.Println(y.Id)

	r, _ := json.Marshal(x)
	fmt.Println(string(r))
}
```

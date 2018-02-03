---
title: 正则表达式的使用
date: 2016-07-08
categories: 编程语言
tags: [PHP, 正则]
---

## 1.什么是正则表达式

人们规定了一些语法规范用来描述一些字符串，比如\s代表空白字符，\d代表数字，.（点）代表任意一个字符，等等

正则表达式就是一串符合上述语法规范的字符串，可以描述出一种规则，进而可以对符合该规则的字符串进行查找或替换等处理：如替换字符串中的大写字母，删除字符串中多余的空白符等。

## 2.常用的语法规范

### 常用字符
字符|说明
--|--
\b | 匹配单词边界，如：'er\b'可以匹配"never"中的'er'，但不能匹配"verb"中的'er'
\B | 匹配非单词边界，如'er\B'能匹配"verb"中的'er'，但不能匹配"never"中的'er'
\d | 匹配数字0-9，等价于[0-9]
\D | 匹配非数字字符，等价于[^0-9]
\w | 匹配包括下划线的任何单字符，等价于[a-zA-Z0-9_]
\W | 等价于[^a-zA-Z0-9]
x｜y | 匹配x或y。例如，z｜food能匹配"z"或"food"。(z｜f)ood则匹配"zood"或"food"。
\num | 其中num是一个正整数,对所获取的匹配的引用。例如，'(.)\1'匹配两个连续的相同字符

### 非打印字符
字符|说明
--|--
\s | 匹配任意空白字符，包括空格、制表符、换页符等。等价于`[\f\n\r\t\v]`
\S | 匹配任意非空白字符。等价于`[^\f\n\r\t\v]`
\f | 匹配换页符
\n | 匹配换行符
\r | 匹配回车符
\w | 匹配字母，数字或者下划线
\W | 匹配所有与`\w`不匹配的字符
\t | 水平制表符
\v | 垂直制表符

### 特殊字符
字符|说明
--|--
$ | 匹配字符串尾，也匹配`\n\r`。匹配本身使用`\$`
() | 标记字表达式开始和结束，匹配本身使用`\(\)`
* | 匹配前面表达式多次或0次，匹配本身使用`\*`
+ | 匹配前面表达式多次或1次，匹配本身使用`\+`
. | 匹配换行符外的任意单个字符，匹配本身使用`\.`
[ | 中括号表达式开始
? | 匹配前面表达式0次或1次
\ | 降下一个字符标记为特殊字符或原意字符或向后引用
^ | 匹配字符开始位置，除非在方括号表达式使用，此时表示不接受该字符集合
{ | 标记限定符表达式的开始
｜ | 指明两项之间的一个选择

### 限定符
字符|说明
--|--
* | 0次或多次
+ | 1次或多次
{n} | n次
{n , } | 至少n次
{n ,m} | 至少n次，至多m次

P.S.：不能对定位符使用限定符。定位符包括

- ^和$（分别表示字符串的开始与结束）
- \b（表示单词的前或后边界）
- \B（表示非单词边界）

### 定界符

定界符中间的字符为描述规则的正则表达式，通常用斜杠/当定界符。如/\d{3}/表示匹配3个数字。 

当表达式中斜杠较多的时候，也可以用其他特殊字符做定界符。如匹配.com结尾的url，正则表达式如下：

```php
#斜杠(/)：
'/http:\/\/.+\.com/'
#波浪号(~)： 
'~http://.+\.com~'
#或井号(#)：
'#http://.+\.com#''
```
上述表达式第一个点表示任意字符，第二个表示点，所以第二个前面要加转义字符反斜杠(\) 。
也可用成对的符号当定界符，如(\d{3})
由于括号，方括号，花括号，竖线，等特殊符号在语法规范中都有特定意义，所以用的时候应当注意。

### 模式修正符
字符|说明
--|--
i | 忽略大小写
m | 将字符串视为多行，不管哪行都能匹配
s | 设此修正符则模式中的圆点字符 . 匹配所有字符包括换行符
e | 设定此修正符则preg_replace()在替换字符串中对逆向引用做正常的替换，将其作为php代码求值，并用其结果来替代所搜索的字符串
D | 设定后，$符号将不匹配换行符换页符
X | 将模式中的空白忽略
A | 强制从目标字符串开头匹配 如/abc/可以匹配"abcde"不能匹配"12abc"
U | 只匹配最近的一个字符串；不重复匹配。如/a.*c/U匹配"abcabbcabbbc"
g | 表示全局匹配

修正符是正则表达式匹配更加精确。 
如/abc/可匹配"abc"但不可匹配"aBc"，但添加修正符i后 /abc/i 就可以匹配；

### 优先级（由高到低）
字符|说明
--|--
\ | 转义符
(),(?:),(?=),[] | 圆括号和方括号
*, +, ?, {n}, {n , }, {n , m} | 限定符
^, $, \anymetacharacter | 位置和顺序
｜ | "或"操作

## 3.php常用的正则函数

### preg_match($pattern, $string [, $arrayname])

该函数返回匹配次数（0或1），因为该函数一旦匹配成功则停止继续匹配，故一般用于做判断验证，即判断某字符串是否符合某要求。如果指定了第三个参数，则将匹配结果赋值给第三个参数，同样只有一个匹配结果。

```php
if (preg_match ("/(\bweb\b)\s(\d)*/i","PHP is the web 45 scripting web 34 language  of choice web . ",$match))
{
	print "A match was found."; 
	print_r($match); 
} else { 
	print "A match was not found.";
}
#结果：A match was found.Array ( [0] => web [1] => web )
```

### preg_match_all($pattern, $string [, $match] [,$flag])

该函数以pattern规则正题解析对比字符串string，匹配结果根据flag指定的方式顺序放在数组match中。函数返回值为匹配的数目，若没有或错误则返回 false 值。 
flag的值有 `PREG_PATTERN_ORDER`（默认值） 及 `PREG_SET_ORDER` 二种。

- `PREG_PATTERN_ORDER`

匹配结果：$match[0]为全部模式匹配的数组，$match[1]为第一个括号中的子模式所匹配的字符串组成的数组，以此类推。

- `PREG_SET_ORDER`

匹配结果：$match[0]为第一组匹配项的数组，$match[1] 为第二组匹配项的数组，以此类推。

```
#例：搜索匹配的html标签
$html = "<b>bold text</b><a href=howdy.html>click me</a>"; 
preg_match_all ("/(<([\w]+)[^>]*>)(.*)(<\/\\2>)/", $html, $matches,PREG_SET_ORDER); 
echo "<pre>";
print_r($matches);

#$flat = PREG_SET_ORDER 结果:
Array
(
    [0] => Array(
            [0] => <b1>example:</b1>
            [1] => example:
        )
    [1] => Array (
            [0] => <div2 align=left>this is a test</div2>
            [1] => this is a test
        )
)

#$flag = PREG_PATTERN_ORDER 结果：
Array
(
    [0] => Array(
            [0] => <b>bold text</b>
            [1] => <a href=howdy.html>click me</a>
        )
    [1] => Array
		(
            [0] => <b>
            [1] => <a href=howdy.html>
        )
    [2] => Array
        (
            [0] => b
            [1] => a
        )
    [3] => Array
        (
            [0] => bold text
            [1] => click me
        )
    [4] => Array
        (
            [0] => </b>
            [1] => </a>
        )
)
```

### preg_replace($pattern , $replacement , $subject [ , $limit])

在subject中搜索pattern模式的匹配并用replacement替换，如果指定limit，则仅替换limit个匹配，若省略limit或者其值为-1，则所有匹配会被置换。

Replacement可以包括`\<n>`形式或`$<n>`形式的逆向引用，推荐用后者。因为对替换模式在一个逆向引用后面紧接着一个数字时（即：紧接在一个匹配的模式后面的数字），不能使用熟悉的 `\1` 符号来表示逆向引用。 
如：`\11`，将会使`preg_replace()`搞不清楚是想要一个`\1`的逆向引用后面跟着一个数字1还是一个`\11` 的逆向引用。
本例中的解决方法是使用`${1}1`。这会形成一个隔离的`$1` 逆向引用，而使另一个1 只是单纯的文字。

如果搜索到匹配项，则会返回被替换后的 subject，否则返回原来不变的 subject。

参数可以是数组也可以是变量，有几种情况：

- 如果subject参数是数组类型。函数对每一个数组元素进行替换操作；
- 如果pattern是数组则函数根据每一个pattern中的类型进行替换；
- 如果pattern和replacement都是数组，则按两个数组中的元素对应完成替换；
- 如果replacement中的元素个数少于pattern中的元素个数。那么不够的部分将有空字符串来代替。
- 
```php
#替换图片
preg_replace('#<img[^>]*sina\.com\.cn/home/c\.gif[^>]*>#', '', $str);
#用getImage函数处理第一个括号中匹配到的字串 [修正符e的描述详见上述常用语法规范]
preg_replace('/<(img.*?)>/ie', "getImage('\\1')", $img_tag);
```

### preg_split($pattern , $subject [, $limit [ ,$flags] ])

该函数返回一个数组，包含subject中沿着与pattern匹配的编辑所分割的字串。

如果指定了limit，则最多返回limit个子串，如果limit 是-1，则意味着没有限制，可以用来继续指定可选参数 flags。

flags 可以是下列标记的任意组合（用按位或运算符 | 组合）：

- `PREG_SPLIT_NO_EMPTY` ：如果设定了本标记，则 preg_split() 只返回非空的成分。
- `PREG_SPLIT_DELIM_CAPTURE` ：如果设定了本标记，定界符模式中的括号表达式也会被捕获并返回。
- `PREG_SPLIT_OFFSET_CAPTURE` ：如果设定了本标记，对每个出现的匹配结果也同时返回其附属的字符串偏移量。注意这改变了返回的数组的值，使其中的每个单元也是一个数组，其 中第一项为匹配字符串，第二项为其在 subject 中的偏移量。

```php
#e.g1
$keywords = preg_split ("/[\s,]+/", "hypertext language, programming"); 
#result
Array ( [0] => hypertext [1] => language [2] => programming )

#e.g2
$str = 'hypertext language programming'; 
$chars = preg_split('/ /', $str, -1, PREG_SPLIT_OFFSET_CAPTURE); 
print_r($chars);
#result
Array(
	[0] => Array(
			[0] => hypertext
			[1] => 0 
			) 
	[1] => Array (
			[0] => language
			[1] => 10
			)
	[2] => Array (
			[0] => programming
			[1] => 19
			)
)
```

P.S.:如果不需要正则表达式的功能，可以选择使用更快（也更简单）的替代函数如 explode() 或 str_split()。

## 4.常用的正则表达式

纯数字：`preg_match('/^\d+$/', $str)`

纯字母：`preg_match('/^[a-z]$/i', $str)`

用户名（字母或下划线开头，字母和数字组成）： 
`preg_match('^[a-z_]+\w*$', $str)`

是否为E-mail： 
`preg_match('/^[_.0-9a-z-]+@([0-9a-z][0-9a-z-]+.)+[a-z]{2,3}$/i', $email)`

获取img标签中的图片：
`preg_match_all('/<img[^>]\s+src=["\']([^"\']*)["\'][^>]*>/is', $str, $match);`


获取a标签中的链接：
` preg_match_all('/<a[^>]\s+href=["\']([^"\']*)["\'][^>]*>/is', $str, $match);`

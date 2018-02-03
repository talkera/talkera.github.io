---
title: PHP正则表达式中的高级用法
date: 2016-07-28
categories: 编程语言
tags: [PHP, 正则]
---

## 1.反向引用
反向引用，也称逆向引用、向后引用，是指在正则表达式内部引用前面被捕获到的内容。

这里有个概念：**捕获** 
捕获：这里的捕获是指将匹配的部分进行储存，可供以后使用。

将一个正则表达式的全部或部分用括号包住，则匹配的过程中会将匹配成功的部分存储到一个临时缓冲区，捕获的每一个匹配都按照在正则表达式中从左到右遇到的内容存储。缓冲区的编号从1开始，直至99。在后面可以通过\1, \2, ${1}这样的形式引用被存储的匹配。 如：
```
//匹配出引号中的内容
$str = <<<EOF
"Double quote" and 'single quote'
EOF;
preg_match_all( "/(\"|').*?\\1/", $str, $match); //此处\\1不能用$1 或者\$1 替代
print_r($match);
//结果
Array
(
    [0] => Array
        (
            [0] => "Double quote"
            [1] => 'single quote'
        )

    [1] => Array
        (
            [0] => "
            [1] => '
        )

)
//或
preg_match_all('/("|\').*?\1/', $str, $match);//此处\1不能用$1替代
```

第一种正则表达式写在双引号中，斜杠后面的字符会被转义，因此反向引用要用\\1。第二种因为表达式写在单引号中，所以用\1。 此外，这里的反向引用都不能使用$1替代，$1用于下面的情况

```
//div去掉属性
$str = <<<EOF
<div style="color:red;">asdasdfasdasdfa</div>
EOF;

echo preg_replace('#<div[^>]*>(.*?)</div>#i', '<div>\1</div>', $str);
echo preg_replace('#<div[^>]*>(.*?)</div>#i', '<div>${1}</div>', $str);
echo preg_replace('#<div[^>]*>(.*?)</div>#i', "<div>\${1}</div>", $str);
```

上述三者效果等同 推荐用${1}，因为如果后面需要跟数字，只有${1}能有效区别开来。 这里的preg_replace有一个很方便的正则使用方式：回调
```
//去掉div标签，并去掉多余空白
$str = <<<EOF
<div style="color:red;">    asdasdfasdasdfa</div>
EOF;

//调用trim函数;注意引号和修正符e
echo preg_replace('#<div[^>]*>(.*?)</div>#ie', "trim('\${1}')", $str);
```
上述正则表达式后追加一个修正符e，则在后面可以跟回调函数。需要注意引号的添加。

## 2.贪婪与非贪婪（懒惰与非懒惰）
贪婪（非懒惰）表示尽可能多的匹配符合规范的字符串，非贪婪（懒惰）表示一旦遇到匹配的即停止。如：
```
// /a.*c/ 对字符串 abcccccccc 的匹配
$str = "abcccccccccccc";

preg_match('/a.*c/', $str, $match);//贪婪模式下可匹配到整个字符串
print_r($match);
//结果
array([0]=>abcccccccccccc)

preg_match('/a.*?c/', $str, $match);//懒惰模式下则只匹配最前面的abc
print_r($match);
//结果
array([0]=>abc)
```

php 正则修正符 U可控制贪婪与非贪婪的逆转，如

```
// /a.*c/ 对字符串 abcccccccc 的匹配
$str = "abcccccccccccc";

preg_match('/a.*c/U', $str, $match);//添加U后，由贪婪转为非贪婪
print_r($match);
//结果
array([0]=>abc)

preg_match('/a.*?c/', $str, $match);//添加U后，由非贪婪转为贪婪
print_r($match);
//结果
array([0]=>abcccccccccccc)
```

在标签匹配中常常遇到此类问题。如匹配并列出各个a标签中的数据<a>.*</a>标签会导致匹配出<a><a/><a><a/><a><a/>这样的字符串，使用<a>.*?</a>才能单独匹配

## 3.关于问号(?)的特殊用法
### 非获取匹配
有时出于某种需要，必须将某部分用圆括号包起来，又不想将其存储起来供以后使用，则需要用到 ?: 来消除圆括号的副作用，不缓存相关匹配。 如： `industr(?:y|ies)` 就是一个比 `industry|industries` 更简略的表达式。

### 正向预查与反(逆)向预查
这个描述有些困难，暂用实例说明
```
'/abc (?=1|2|3)/'
#能匹配 "abc2asd" 中的 "abc" ，但不能匹配 "abcasd" 中的 "abc"。
#因为要求abc后面必须是1或2或3。

'/abc (?!1|2|3)/'
#不能匹配 "abc2asd" 中的 "abc" ，但能匹配 "abcasd" 中的 "abc"。
#因为要求abc后面不能使1或2或3。
```
上述被称为正向预查。?后面的 '='和'!' 分别表示'是'和'非'

反向预查表示向前判断
```
'/(?<=1|2)abc/ '
#能匹配 '2abcd '中的 abc， 但是不能匹配 3abcd 中的 abc
'/(?<!1|2)abc/'
#不能匹配 '2abcd '中的 abc， 但是能匹配 3abcd 中的 abc
```
以上用到的四个符号分别是 ?=, ?! ?<=, ?<!。其中<表示向左即向前，=表示'是'，!表示'非'

> 正反向预查使用括号都是**非获取匹配**，匹配结果不会被缓存。并且，**预查都不消耗字符串**。如： `'/abc (?=1|2|3)/'` 匹配 abc2asd 的 abc 之后， 下一次匹配从2开始，而不是从2之后的a开始

---
title: Yii中的过滤器-Filter
date: 2016-06-17
categories: 编程语言
tags: [PHP, yii]
---

在网上找关于Yii的过滤器内容实在太少，而且大多数讲得不得要领。只好读源码来理解。现把笔记整理如下：

<!--more-->

正常情况下，访问某个`action`时，yii会通过调用`CController`的 `runActionWithFilters` 方法来执行`action`。核心代码如下：

```php
//CController.php CController是所有controller的基类
//$filters来自于当前 controller 中的 filters 方法

$filterChain = CFilterChain::create($this,$action,$filters);
$filterChain->run();
```

即：将当前`controller`对象 + `action`对象 + `$filters` 一起传递给`CFilterChain`的`create`方法，创建一个过滤器的链[filter-chain]--`$filterChain`。然后执行该 `$filterChain` 的 `run()` 方法。

在此并没有看到执行 `action` 部分的代码，因为执行`action`的代码在 `$filterChain` 的 `run()` 方法中实现。

## 过滤器链-FilterChain

上述用到的变量`$filterChain` 是一个 `CFilterChain` 对象，本质上是一个`List` 对象（`CList`），一个链式结构。

该对象包含一个数组，数组元素是一个个`filter`对象（`CFilter`）。另外包含一个游标，用来保存当前指向的filter下标。

下面是`CFilterChain`的 `run()` 方法

```php
public function run()
{
	if($this->offsetExists($this->filterIndex)) //如果当前游标有指向的 filter
	{
		$filter=$this->itemAt($this->filterIndex++); //获取当前 filter 对象, 并将游标 +1
		$filter->filter($this); //执行该filter对象的filter方法，执行时需将当前的$filterChain作为参数
	}
	else
		$this->controller->runAction($this->action); //当前游标的指向没有 filter对象 时执行action
}
```

该 `run()` 方法完成两件事：

1.  如果有未执行的`filter`则执行`filter $filter->filter($this)`
2.  执行完最后一个`filter`后执行`action()`

因此，每个 `filter` 对象 必须有`filter()` 方法。每个 `filter` 在完成业务逻辑后，要继续调用 `$filterChain` 的 `run()` 方法。直到最后一个 `filter` 通过后，执行 `action`。

## Yii中的filter

当前`controller`中，各个action需要用到的`filter`都在`controller`的`filters()`方法中定义。即：`runActionWithFilters()`方法中的`$filters`变量的值通过调用当前`controller`的`filters()`方法获得。

Yii中可调用的`filter`分两类，在`controller`中分别以字符串和数组的方式调用。两类`filter`在`controller`中的调用方法如下：

```
public function filters(){
	return array(
		'postOnly - refresh', //第一种filter，以字符串定义
		array('path.to.filterClass - index', 'param1'=&gt;'value1'), //第二种filter 以数组定义
	);
}
```

在创建`CFilterChain`对象时，会将`filters()`方法中定义的不同`filter`分别以不同的方式生成。以字符串调用的会生成`CFilterInline`对象；以数组调用的，必须是已定义的类，继承自`CFilter`的类，会被yii以创建组件的方式创建为对象，本质上为使用new关键字。

### CInlineFilter

这是第一类，以字符串方式调用的`Filter`，会生成`CInlineFilter`对象。如常见的`postOnly`，`accessControl`等。这类`filter`的 `filter()`方法实现很简单，本质上是调用当前`controller`中以`filter`开头的方法，如：

```php
//CInlineFilter.php
public function filter($filterChain)
{
	$method='filter'.$this->name;
	$filterChain->controller->$method($filterChain);
}
```

该方式要求对应的`filter`方法中必须有调用`$filterChain->run()`的逻辑操作，如 `postOnly` 的实现：

```php
//CController.php
public function filterPostOnly($filterChain)
{
	if(Yii::app()->getRequest()->getIsPostRequest())
		$filterChain->run(); //在这里完成filterChain的继续
	else
		throw new CHttpException(400,Yii::t('yii','Your request is invalid.')); //不通过则报错，不再执行后面的filter
}
```

### Filter类

这是第二类以数组方式调用的`Filter`。本质上为继承自`CFilter`的类。只要该类实现的`preFilter`方法即可。父类（`CFilter`）中的`run`方法会调用 `$filterChain->run()`，如：

```
//CFilter.php
public function filter($filterChain)
{
	if($this->preFilter($filterChain))
	{
		$filterChain->run();
		$this->postFilter($filterChain);
	}
}
```

## preFilter和postFilter

每个Filter类都包含preFilter和postFilter。FilterChain中的`filter`执行顺序一般为：
```
filter1.preFilter -> filter2.preFilter -> ... filterN.preFilter -> actioin -> filterN.postFilter -> ... filter2.postFilter -> filter1.postFilter
```
因此为了在`filter`中进行访问控制，一般都在`preFilter()`方法中写过滤逻辑。如果写到`postFilter()`，等到执行的时候`action`已经执行完毕了。

## 常用filter及定义方式

最常用到的`filter`就是yii自带的三个`CInlineFilter`: `postOnly` `ajaxOnly` `accessControl`，调用的操作如下：

```
public function filters(){
	return array(
		//"-"表示：除了refresh的action外，本`controller`中所有操作只能以post请求进行
		'postOnly - refresh',

		//"+"表示：disable 和enable 操作只能以ajax方式进行
		'ajaxOnly + disable,enable',

		//没有"-"和"+"表示：所有操作都要通过 accessControl 过滤器
		'accessControl',
	);
}
```

如上述所述，三个`filter`都是`CInlineFilter`，本质上是调用`CController`中定义的对应的`filter`方法：`filterPostOnly` `filterAjaxOnly` `filterAccessControl`。

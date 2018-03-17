---
title: Yii的访问控制——accessControl
date: 2016-06-26
categories: 编程语言
tags: [PHP, yii]
---

网站某些页面并非无限制访问，有些需要登录，有些需要高级用户权限。因此一套良好的访问控制机制必不可少。 
Yii自带访问控制的功能，只需少许配置就能使用。

<!--more-->

进行访问控制前，要先了解Yii的filter和RBAC。因为访问控制基本都是建立在这两者的基础上。
{% post_link yii-RBAC  [查看Yii的RBAC介绍] %}
{% post_link yii-filter  [查看Yii的Filter] %}

Yii自带的访问控制的入口为CController中的一个方法 - filterAccessControl。 
使用时只需要在当前controller的filters方法内配置即可。如：

```
public function filters(){
	return [
		//... 其他过滤器
		'accessControl',//访问控制过滤器
	];
}
```

使用该filter还需要配置对应的访问控制规则，这些规则定义在当前controller内的accessRule方法中。如：

```
public function accessRule(){
	return [
		['allow', 'actions'=>['index','view'], 'users'=>'@'],
		['allow', 'actions'=>['create', 'update'], 'roles'=>['editor']],
		['allow', 'actions'=>['delete'], 'users'=>'sudobash', 'verb'=>'post'],
		['deny', 'actions'=>'*'],
	];
}
```

上述代码定义的规则分别是：

1.  登录后的用户（@）可以访问 actionIndex，actionView
2.  角色是editor的用户可以访问 actionCreate，actionUpdate
3.  用户名是sudobash的用户 并且 是post请求 才可以访问actionDelete
4.  不能访问任何action 该访问规则按照从上到下的顺序依次判定。 上述访问规则的实现逻辑代码在framework/web/auth/CAccessControlFilter.php中。

## 根据角色限制

对于用户是否登录、以及用户名的判定都由CAccessControlFilter直接实现。 
对于角色的限制，则是调用了CWebUser的checkAccess()方法。 
而CWebUser中checkAccess的方法，本质还是调用Yii::app()->authManager->checkAccess()的方法（见CWebUser.php）。

因此如果有根据角色进行限制的需求，需要先通过authManager进行操作。如：

```
$authManager = Yii::app()->authManager;
$authManager->createRole('editor');
$authManager->assign('editor', 'userId');
```

在此，角色只是作为一个授权单位（AuthItem）来使用的。 
检查用户是否拥有该角色的判断，本质上是检查用户是否有该AuthItem的授权。
 因此该AuthItem不仅限于角色，也可以使用operation或task:

```
$authManager->createOperation('opName');
$authManager->createTask('taskName');
```

此外 accessControl 还可以进行其他方面的限制。如:

```
//执行一段自己定义的php代码：让admin用户拥有所有访问权限
['allow', 'actions'=>'*', 'expression'=>'return Yii::app()->user->name=="admin"'],

//设定IP段，该网段的IP可以访问
['allow', 'actions'=>'*', 'ips'=['111.111.111.*']]
```

其他用法参见CAccessControlFilter.php

---
title: Yii的授权-RBAC
date: 2016-06-18
categories: 编程语言
tags: [PHP, yii]
---


Yii的RBAC隐藏得比较深，而且直接用不太好用。
 但是由于其基本功能很完善，因此只需后续少许开发就能真正使用，而且非常好用。

<!--more-->

## 授权单位 - AuthItem

一个AuthItem就是一个做某件事的许可。
 Yii中设定了不同粒度的AuthItem：operation（操作）、task（任务）、role（角色），一般来说：

1.  Operation是最小的，不可再分的AuthItem 
2.  Task是一个或多个operation组成的AuthItem 
3.  Role是一个或多个task组成的AuthItem

Yii中并未对三类AuthItem做强行限制，因此实际上有时候一个role也可能由operation组成，甚至由operation和task混合组成。
 但Yii做了另外的限制&mdash;&mdash;小粒度不能包含大粒度的子项，即：一个task不能包含role子项，一个operation也不能包含task或role子项。

## AuthManager

有了AuthItem，就要有授权操作（assign）和权限检查（checkAccess）。
Yii中权限的授予（assign）、判定（checkAccess）、撤销（revoke）的操作，以及创建AuthItem的操作都依赖于authManager。

一个Yii的webapp中必定有一个authManager的组件。该组件是一个继承自CAuthManager的一个类。这个类要能实现以下功能：

1.  AuthItem（即operation、task、role）的增删改查 
2.  授权关系（assignment）的增删改查 
3.  AuthItem父子关系的增删改查

Yii自带了两个实现以上功能的类：CPhpAuthManager类和CDbAuthManager类。
它们的区别在于CPhpAuthManger将授权数据都保存到一个php文件中，而CDbAuthManager将授权数据都保存到数据库的三个表中。

然而无论是保存到数据库的表，还是保存到文件，Yii都不会自动生成表和文件。都需要手动去初始化，去处理，后续还需要维护。

在不进行任何配置时，Yii默认使用CPhpAuthManager，以文件的形式进行权限控制。 
文件路径为：

```
Yii::getPathOfAlias('application.data.auth').'.php';
//一般为 protected/data/auth.php
```

## 授权 - assign

为用户授权的操作（assignment），实际上是保存一个授权单位（AuthItem）和用户标识（userid）的关联关系。因此一个assignment中必然包含一个AuthItem和一个userid。

以具体举例分析：
现在为创建文章（createPost）的操作创建一个operation。将整个文章管理（postManagement）功能当做一个task。将createPost当做postManagement的子项。
然后把postManagement的权限授予给userid为Tyler的用户。

在siteController中创建如下action：

```php
public function actionRights(){
	$authManager = Yii::app()->authManager;//CPhpAuthManager对象
	$operation = $authManager->createOperation('createPost');
	$task = $authManager->createTask('postManagement');
	$task->addChild('createPost');
	$authManager->assign('postManagement', 'Tyler');
	$authManager->save();
}
```

由于并没有配置AuthManager，因此Yii使用的是CPhpAuthManager。 
该action最后一步将授权信息落地到auth.php文件中。 
因此访问该action后，可看到授权文件内容如下：

```php
<?php
return array (
  'createPost' => 
  array (
    'type' => 0,
    'description' => '',
    'bizRule' => NULL,
    'data' => NULL,
  ),
  'postManagement' => 
  array (
    'type' => 1,
    'description' => '',
    'bizRule' => NULL,
    'data' => NULL,
    'children' => 
    array (
      0 => 'createPost',
    ),
    'assignments' => 
    array (
      'Tyler' => 
      array (
        'bizRule' => NULL,
        'data' => NULL,
      ),
    ),
  ),
);
```

## 权限校验 - checkAccess

当Tyler访问创建文章的操作时，可通过authManager进行检查。如：

```
//PostController.php
public function actionCreate(){
	$authManager = Yii::app()->authManager;
	$user = Yii::app()->user;

	//判断Tyler是否有createPost的权限
	if(!$authManager->checkAccess('createPost', $user->id)){
		throw new CHttpException(403, 'forbidden');
	}
}
```

代码执行流程如下：

1.  根据当前登录用户的标识（userid）取出该用户所有授权（assignments） 
2.  判断该AuthItem是否在该用户的授权列表中，如果存在，则权限校验通过。 
3.  如果不在授权中，则获取该AuthItem的父级AuthItem，判断父级AuthItem是否在授权中。如果在，则权限校验通过。不在则继续向上，判断各个父级AuthItem的父级AuthItem是否在用户授权中。 
4.  直到找不到更高级的AuthItem，则说明权限校验不通过。

即：根据Tyler的assignments取出所有与Tyler有关联关系的AuthItem。可以取到postManagement。 
然后判断createPost不在assignments中因此校验不通过。 
继续找createPost的父级，可找到postManagement，在assignments中，因此最终校验通过。

具体实现代码见：CPhpAuthManager.php的checkAccess()方法 及 CDbAuthManager.php的checkAccessRecursive()方法。

Yii的CWebUser对checkAccess又封装了一层。 
Yii::app()->user->checkAccess('createPost'); 基本等同于 Yii::app()->authManager->checkAccess('createPost', Yii::app()->user->id);

## CDbAuthManager

如果用数据库来存储授权数据，需要先建表。
 Yii自带了建表的sql，该文件在 framework/web/auth/schema.sql 该结构由三个表组成：

*   AuthItem-存所有的储授权单位：role，task，operation
*   AuthChild-存储AuthItem之间的父子关系
*   AuthAssignment-存储授权记录：AuthItem，userId的关联关系

虽然数据管理权限和文件管理权限的存储结构以及实现方式有所不同，但两者功能以及操作方式相同。

另外，authManager还有其他的操作，如：

```
$authManager->createRole('editor');//创建角色
$authManager->revoke('editor', 'Tyler');//取消授权
$authManager->getAuthAssignments('Tyler');//获取用户所有授权
//...
```

权限的设定和检查基本操作就是这样。然而这样的权限管理是没办法直接应用于生产环境的：

1.  AuthItem的管理操作要靠写代码
2.  assignment的管理操作要靠代码
3.  权限检查的操作太混乱，维护成本极高

因此最好的解决的办法是：单独开发一套用于权限管理的模块，并且把权限验证做成自动化的。 
目前已有现成的第三方module&mdash;&mdash;rights。
 理解了Yii的RBAC原理，可以更好的使用rights。

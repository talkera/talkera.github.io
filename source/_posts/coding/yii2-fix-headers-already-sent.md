---
title: Yii2 中解决 header already send错误
date: 2018-03-14
categories: 工作点滴
tags: [yii2,php]
---

先上解决方案，后面是复现场景。

## 解决方案
用yii2自己的响应器来管理header和输出内容。
```php
$response = Yii::$app->getResponse();
$response->getHeaders()->add('Content-Type', 'application/json;charset=utf-8');
$response->content = json_encode($responsData);
Yii::$app->end();
```

## 场景

用yii2开发的API，在frontend中新建了一个module，将错误捕获也放在了module中。

```php
class ApiModule extends \yii\base\Module
{
	public function init()
	{
		parent::init();

		\Yii::$app->errorHandler->errorAction = '/v1/default/error';
	}
}
class DefaultController extends IController{
	public function actionError(){
		if (($exception = \Yii::$app->getErrorHandler()->exception) === null) {
			$exception = new \yii\web\NotFoundHttpException(Yii::t('yii', 'Page not found.'));
		}
		$this->jsonOut(1001, $exception->getMessage());
	}
}
class IController extends yii\web\Controller{
	public function json($arr=array()){
		header("Content-Type: application/json;charset=utf-8");
		echo json_encode($arr);
		Yii::$app->end();
	}
}
```

本来希望捕获错误后输出json结果后就结束。可是结果后面总是跟着一堆exception，提示`Headers already sent`。 不仅如此，连带被捕获的错误也报出来了。如下：
```
An Error occurred while handling another error:
exception 'yii\web\HeadersAlreadySentException' with message 'Headers already sent in D:\path\to\project\common\components\IController.php on line 43.' in D:\path\to\project\vendor\yiisoft\yii2\web\Response.php:366
Stack trace:
#0 D:\path\to\project\vendor\yiisoft\yii2\web\Response.php(339): yii\web\Response->sendHeaders()
#1 D:\path\to\project\vendor\yiisoft\yii2\base\Application.php(656): yii\web\Response->send()
#2 D:\path\to\project\common\components\IController.php(44): yii\base\Application->end()
#3 D:\path\to\project\common\components\IController.php(49): common\components\IController->json(Array)
#4 D:\path\to\project\frontend\modules\v1\controllers\DefaultController.php(14): common\components\IController->jsonOut(1001, '\xE9\xA1\xB5\xE9\x9D\xA2\xE6\x9C\xAA\xE6\x89\xBE\xE5\x88\xB0...')
#5 [internal function]: app\modules\v1\controllers\DefaultController->actionError()
#6 D:\path\to\project\vendor\yiisoft\yii2\base\InlineAction.php(57): call_user_func_array(Array, Array)
#7 D:\path\to\project\vendor\yiisoft\yii2\base\Controller.php(157): yii\base\InlineAction->runWithParams(Array)
#8 D:\path\to\project\vendor\yiisoft\yii2\base\Module.php(528): yii\base\Controller->runAction('error', Array)
#9 D:\path\to\project\vendor\yiisoft\yii2\web\ErrorHandler.php(108): yii\base\Module->runAction('/v1/default/err...')
#10 D:\path\to\project\vendor\yiisoft\yii2\base\ErrorHandler.php(111): yii\web\ErrorHandler->renderException(Object(yii\web\NotFoundHttpException))
#11 [internal function]: yii\base\ErrorHandler->handleException(Object(yii\web\NotFoundHttpException))
#12 {main}
Previous exception:
exception 'yii\base\InvalidRouteException' with message 'Unable to resolve the request: v1/default/' in D:\path\to\project\vendor\yiisoft\yii2\base\Controller.php:128
Stack trace:
#0 D:\path\to\project\vendor\yiisoft\yii2\base\Module.php(528): yii\base\Controller->runAction('', Array)
#1 D:\path\to\project\vendor\yiisoft\yii2\web\Application.php(103): yii\base\Module->runAction('v1/', Array)
#2 D:\path\to\project\vendor\yiisoft\yii2\base\Application.php(386): yii\web\Application->handleRequest(Object(yii\web\Request))
#3 D:\path\to\project\frontend\web\index.php(17): yii\base\Application->run()
#4 {main}

Next exception 'yii\web\NotFoundHttpException' with message '页面未找到。' in D:\path\to\project\vendor\yiisoft\yii2\web\Application.php:115
Stack trace:
#0 D:\path\to\project\vendor\yiisoft\yii2\base\Application.php(386): yii\web\Application->handleRequest(Object(yii\web\Request))
#1 D:\path\to\project\frontend\web\index.php(17): yii\base\Application->run()
#2 {main}
```

于是推断是`echo`之后，`Yii::$app->end()`中再次发送了header，所以触发报错，将exception的栈都连带出来了。于是不直接输出，而是将结果交给yii自带的响应器。

```php
class IController extends yii\web\Controller{
	public function json($arr=array()){
		$response = Yii::$app->getResponse();
		$response->getHeaders()->add('Content-Type', 'application/json;charset=utf-8');
		$response->content = json_encode($arr);
		Yii::$app->end();
	}
}
```
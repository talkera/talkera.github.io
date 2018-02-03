---
title: PHP中session的保存
date: 2016-09-08
categories: 编程语言
tags: [PHP, session]
---
ini中关于session的配置一般为

```
session.save_handler = "files"
session.save_path = "/tmp/session"
```

session默认以文件的形式保存在服务器。用session.save_path来设置保存session文件的路径. 
也可以在php代码中通过session_save_path()函数来获取或设置保存路径。 如：

```
session_save_path("/tmp/session");
```

也可以通过ini_set设置session的相关配置。 如：

```
ini_set('session.save_path','/tmp/session');
```

现在由于多服务器部署，以文件保存的session只能各自保存在各个前端服务器上，无法共享。 因此现在一般将session保存到memcache、redis、数据库等可共享的服务中。

## ini中设置保存方式

如保存在redis中时

```
session.save_handler = redis
session.save_path = "tcp://127.0.0.1:6379"
session.save_path = "tcp://127.0.0.1:6379?user=password" #有密码的redis
#保存在redis中的key为 "PHPREDIS_SESSION:".session_id();
```

如保存在memcache中时：

```
session.save_handler = memcache
session.save_path = "tcp://127.0.0.1:11211"
#保存在memcache中的key为session_id()
```

在php.ini中的配置也可通过ini_set函数来设置。 这种通过ini配置的方法操作简单，适合小规模项目使用。

## 使用 session_set_save_handler

session的相关操作有 开启，关闭，读操作，写操作，销毁，过期回收 可以将这些操作写成自定义函数或类的方法，然后通过 session_set_save_handler 设置session的操作由自定义的函数来实现即可。 如：在数据库中保存session

```php
class DBSession{
	private $_c;
	public function open($savePath, $saveName){
		$this->_c = mysql_connect('127.0.0.1', 'root', 'root');
		mysql_select_db('test');
		return true;
	}
	public function close(){
		mysql_close();
		return true;
	}
	public function write($sessionId, $data){
		$sql = "insert into session(`sessionId`, `data`, `createTime`) values ('{$sessionId}','{$data}','".time()."')";
		mysql_query($sql);
		return true;
	}
	public function read($sessionId){
		if($res = mysql_query("select data from session where sessionId='{$sessionId}'")){
			if($row = mysql_fetch_row($result)) return $row['data'];
		}
		return '';
	}
	public function destroy($sessionId){
		mysql_query("delete * from session where sessionId='{$sessionId}'");
		return true;
	}
	public function gc($maxLifeTime){
		$toDelete = time()-$maxLifeTime;
		mysql_query("delete from session where createTime<'{$toDelete}'");
		return true;
	}
}
$sessionHandler = new DBSession;
session_set_save_handler(
	[$sessionHandler, 'open'],
	[$sessionHandler, 'close'],
	[$sessionHandler, 'read'],
	[$sessionHandler, 'write'],
	[$sessionHandler, 'destroy'],
	[$sessionHandler, 'gc']
);
```

php5.4以上可自己写类继承SessionHandlerInterface，实现该接口定义的方法。

```php
class DBSession implements SessionHandlerInterface{
	public function open(){}
	public function close(){}
	public function read(){}
	public function write(){}
	public function gc(){}
	public function destroy(){}
}
session_set_save_handler(new DBSession, true);
```

## 总结：

更推荐使用 session_set_save_handler 来控制session保存，原因如下：

1.  直观明了。如果在ini中配置，可能只有运维或有一定权限的人才能看到。 
2.  修改方便，如果在ini中配置，一旦修改则需要重启php-fpm或Apache。修改代码一般只需一次部署即可。 
3.  学习成本低，一次学习，多重使用方式。在ini中配置mysql、memcache、redis各不同，可能还需要去了解各种保存方式使用的key。 开发以实现需求为目的，不断学习也是为了实现需求。但应该尽可能避免不必要的学习。毕竟时间有限。 
4.  使用灵活。在多服务器、一致性hash、memcache加密等方面，设置灵活。

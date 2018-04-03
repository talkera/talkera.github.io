---
title: 使用脚本与以太坊交互
date: 2018-04-03
categories: 区块链
tags: 
	- 区块链
	- 以太坊
---

除了控制台外，以太坊还支持用脚本与geth交互。除了JS外，还支持Python、PHP Java等。

<!--more-->

## 以太坊的交互方式

与以太坊的交互有好几种：
- geth控制台
- mist钱包
- node命令行
- node运行js脚本
- php 、Python and so on

然而归根到底就分两种：在geth控制台中交互 或者 通过其他方式与geth交互。

geth集成了node的控制台，并且封装了`web3.js`的类库，用于操作本地的以太坊节点或与远程节点通信。因此js开发可以直接引用web3的类库。然后又陆续开发了其他语言的web3类库，相当于开放的`sdk`或者类库，只要引用就能用之与以太坊节点交互，交互对象就是geth提供的`rpc`服务。

geth本身支持`rpc`、`websocket`等通讯方式，本文介绍的js使用的是`rpc`方式。

> web3寓意web3.0，旨在为互联网更新换代，实现价值互联……真是雄心壮志

## JS与geth交互

JS交互需要搭建node环境，**尤其是环境变量的设置**.参考 {% post_link coding/node-env-2018 %}

geth内部集成了node的控制台，在以太坊上的操作大多数操作都是通过web3模块来进行的，按理说js与node的交互应当是水到渠成的。然而node的web3安装却磨难重重。

{% note warning %} geth内部集成的web3是0.2版，但是npm 默认安装的是1.0版本的，1.0还没有release 所以不指定版本安装可能会出现各种各样的问题！ {% endnote %}

鉴于`1.0`各种安装出错，并且跟geth不一致，还是装`0.2`版本比较好。mac和win下都能正常安装，唯独linxu下麻烦不断。

{% note danger %} 最终发现linux下的web3要用普通用户进行全局安装，不能用root。 如果权限受限，要执行chmod 777 $NODE_PATH {% endnote %}

```bash
#安装web3
npm intall -g web3@^0.20.1

#运行一个geth孤点 开启rpc和控制台
#如果不适用metamask部署而是直接部署，启动geth要去掉--nodiscover参数
geth --nodiscover console --rpc --rpcapi 'web3,eth,debug' --rpcport 8545 --rpccorsdomain '*'
```
web3成功的标志是：**进入node控制台，能require到web3**

编写 `interact.js`
```js
//获取web3对象 
var web3 = require('web3')
web3 = new web3()
web3.setProvider( new web3.providers.HttpProvider('http://127.0.0.1:8545') )
var eth = web3.eth

//geth中自动操作了上面的步骤，所以如果在geth中直接可以使用web3和eth
//以下步骤在geth中操作和使用node执行js效果是一样的

var myAcc = eth.accounts[0] //用于部署合约的账户

//然后就可以使用 abi + bytecode 部署合约了
var abi = [{....}]
var bytecode = '0xsssssssssss'
var contractParam1,  contractParam2 //智能合约构造函数的参数

//可以生成部署的操作码 在用metamask广播部署
var contractCode = eth.contract(abi).new.getData(contractParam1, contractParam2, {
	from:myAcc, 
	data:bytecode
)

//有条件的可以直接部署。（有条件是指可以使用eth.getBalance()可以查到足够的余额）
var contract = eth.contract(abi).new(contractParam1, contractParam2, {
	from:myAcc, 
	data:bytecode
)
```

使用node执行该文件
```bash
$ node interact.js
```
更多API参考见 [`0.2`版本文档](https://github.com/ethereum/wiki/wiki/JavaScript-API)

## PHP与geth交互
php要7.1 

Github:https://github.com/sc0Vu/web3.php

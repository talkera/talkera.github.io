---
title: 使用脚本与以太坊交互
date: 2018-03-31
categories: 区块链
tags: 
	- 区块链
---

除了控制台外，以太坊还支持用脚本与geth交互。除了JS外，还支持Python、PHP Java等。

{% note warning %} 然而php要7.1 node安装web3不一定成功 {% endnote %}

<!--more-->

## PHP与geth交互
Github:https://github.com/sc0Vu/web3.php


## JS与geth交互

geth内部集成了node的控制台，在以太坊上的操作大多数操作都是通过web3模块来进行的，按理说js与node的交互应当是水到渠成的。然而node的web3安装却遭遇重大挫折，目前只在mac和windows上安装成功，centos6.8和centos7都失败了。

{% note warning %} npm 默认安装的是1.0版本的 {% endnote %}

另外geth控制台和外部脚本操作略有区别：geth集成的web3模块是`0.20.1`版本的，而npm安装的是`1.0`版本的，版本更高，文档也略有差别，本文以`1.0`版本为准。
- 0.2版本文档：https://github.com/ethereum/wiki/wiki/JavaScript-API
- 1.0版本文档：http://web3js.readthedocs.io/en/1.0/index.html


```bash
#安装web3
npm intall -g web3@^0.20.1

#运行一个geth孤点 开启rpc和控制台
geth --nodiscover console --rpc --rpcapi 'web3,eth,debug' --rpcport 8545 --rpccorsdomain '*'
```

web3成功的标志是：**进入node控制台，能require到web3**
```bash
$ node
> require('web3').version
'1.0.0-beta.33'
```

{% note warning %} 如果web3安装不成功，就不要继续了 {% endnote %}

interact.js
```
var fs = require("fs")
var web3 = require('web3');
web3 = new web3();
web3.setProvider( new web3.providers.HttpProvider('http://127.0.0.1:8545') )

var eth = web3.eth

console.log(eth.accounts)

var api = [{"constant":false,"inputs":[{"name":"_initialAmount","type":"uint256"},{"name":"_name","type":"string"},{"name":"_decimals","type":"uint8"},{"name":"_symbol","type":"string"}],"name":"createEIP20","outputs":[{"name":"","type":"address"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"","type":"address"},{"name":"","type":"uint256"}],"name":"created","outputs":[{"name":"","type":"address"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"","type":"address"}],"name":"isEIP20","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"EIP20ByteCode","outputs":[{"name":"","type":"bytes"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"_tokenContract","type":"address"}],"name":"verifyEIP20","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"inputs":[],"payable":false,"stateMutability":"nonpayable","type":"constructor"}]

var add = '0x0f85579751197DC71A1fFf9089892558b7F62005'

var cInstance = eth.contract(abi).at(add)

var myAcct = eth.accounts[0]
var othersAcct = eth.accounts[1]

console.log(cInstance.balanceOf(myAcct))

eth.defaultAccount = myAcct
console.log(cInstance.transfer.getData(othersAcct), 1000)
```

## 其他资料
- Github-web3: https://github.com/ethereum/web3.js
- Github-web3.php: https://github.com/sc0Vu/web3.php
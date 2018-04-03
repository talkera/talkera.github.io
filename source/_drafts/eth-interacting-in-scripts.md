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

JS交互需要搭建node环境，尤其是环境变量的设置.参考 {% post_link coding/node-env-2018 %}

geth内部集成了node的控制台，在以太坊上的操作大多数操作都是通过web3模块来进行的，按理说js与node的交互应当是水到渠成的。然而node的web3安装却磨难重重。

{% note warning %} geth内部集成的web3是0.2版，但是npm 默认安装的是1.0版本的，1.0还没有release 所以不指定版本安装可能会出现各种各样的问题！ {% endnote %}

- 0.2版本文档：https://github.com/ethereum/wiki/wiki/JavaScript-API
- 1.0版本文档：http://web3js.readthedocs.io/en/1.0/index.html

鉴于`1.0`各种安装出错，并且跟geth不一致，还是装`0.2`版本比较好。mac和win下都能正常安装，唯独linxu下略麻烦。

{% note warning %} linux要用普通用户进行全局安装，不能用root。 如果权限受限，要执行chmod 777{% endnote %}

```bash
#安装web3
npm intall -g web3@^0.20.1

#运行一个geth孤点 开启rpc和控制台
geth --nodiscover console --rpc --rpcapi 'web3,eth,debug' --rpcport 8545 --rpccorsdomain '*'
```

web3成功的标志是：**进入node控制台，能require到web3**

interact.js
```
var web3 = require('web3');
web3 = new web3();
web3.setProvider( new web3.providers.HttpProvider('http://127.0.0.1:8545') )

var eth = web3.eth

console.log(eth.accounts)

var abi = [{"constant":false,"inputs":[{"name":"_initialAmount","type":"uint256"},{"name":"_name","type":"string"},{"name":"_decimals","type":"uint8"},{"name":"_symbol","type":"string"}],"name":"createEIP20","outputs":[{"name":"","type":"address"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"name":"","type":"address"},{"name":"","type":"uint256"}],"name":"created","outputs":[{"name":"","type":"address"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"","type":"address"}],"name":"isEIP20","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"EIP20ByteCode","outputs":[{"name":"","type":"bytes"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[{"name":"_tokenContract","type":"address"}],"name":"verifyEIP20","outputs":[{"name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"inputs":[],"payable":false,"stateMutability":"nonpayable","type":"constructor"}]

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
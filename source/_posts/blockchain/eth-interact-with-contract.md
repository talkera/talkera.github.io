---
title: 以太坊与智能合约交互
date: 2018-04-03
categories: 区块链
tags: 
	- 区块链
	- 以太坊
---

与智能合约的交互首先要获取合约实例，然后调用合约方法：本地调用 或 广播.

本文中的合约和方法均见于ERC20。

<!--more-->

## 获取合约实例

在geth终端或者通过执行脚本部署合约时，立刻就能获取到合约实例。
```js
var abi = [{...}]
var bytecode = '0xbbbbbbbbbb'
var myAcct = eth.accounts[0]

//部署直接获取实例
c = web3.eth.contract(abi).new({data:bytecode, from:myAcct});
```

如果已经部署过的合约，则需要`ABI`和合约地址

下面的操作可以在geth中操作，也可以使用js脚本 node终端。操作区别见 {% post_link blockchain/eth-interacting-in-scripts %}
```js
var abi = [{...}]
var add = '0xsssssss'
var c = eth.contract(abi).ad(add) //获取合约实例
```

## 执行合约方法
获取合约实例后可执行合约方法。根据方法内是否有写操作，执行分两种：
- 本地执行
- 广播执行

第一种一般用于查询操作，比如查询余额。另外由于是本地执行，所以要求本地**同步到相关的最新块才能查到最终结果**。比如这个合约最后一笔转账是一小时前，而数据只同步到到昨天，那么查到的结果只是昨天的结果，而不是最终结果。
```js
c.balanceOf(eth.accounts[0])
```

第二种用于写操作，比如转账。因为是transaction所以必须要支付gas。支付的gas从`eth.defaultAccount`中扣，因此这个账户必须设置并且要能用`eth.getBalance()`查到足够用的eth余额。

另外使用前要先将账户解锁。**解锁操作只能在geth中执行**

{% note danger %} 如果未设置`eth.defaultAccount`会报错：`Error: invalid address` {% endnote %}

```js
//解锁后才能支付gas
personal.unlockAccount(eth.defaultAccount)
eth.defaultAccount = eth.accounts[0]
c.transfer(eth.accounts[1], 1000)
```

### 生成操作码
如果查不到eth余额则可以采取另一种方式：生成操作码，在matamask广播。

生成操作码的方法是：`c.methodName.getData()` 函数中传入原本方法需要的参数。如转账：
```js
c.transfer().getData(eth.accounts[1], 1000)
```

合约中的方法都可以生成操作码，**包括查询余额**，只不过很不划算。
```js
c.balanceOf.getData('0x88eE7105738aA1cf2359278974E68830CA7f9be3')
```

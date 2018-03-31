---
title: 以太坊发token最佳实践
date: 2018-03-30
categories: 区块链
tags: 
	- 区块链
	- 以太坊
	- token
	- 代币
---

发代币历经波折，现在将过程记录如下。win适用，mac/linux更适用。

必备技能：1.科学上网条件。2.会使用npm 3.有足够的以太坊（0.2eth足矣）

<!--more-->

发token其实是部署智能合约。要经历4个步骤：
- 搭建编译环境
- 编写智能合约
- 编译合约
- 部署合约
- 与合约交互

## 准备工作
- 科学上网条件
- chrome浏览器，安装metamask插件：https://metamask.io/
- node环境，npm命令，本文使用的是淘宝技术团队提供的cnpm
- 下载geth： https://geth.ethereum.org/downloads/

## 搭建编译环境
写智能合约用的最多的是solidity语言，然而编译环境实在是懒得吐槽。有在线编译，有框架编译。不过最推荐的是**搭建在本地的在线编译**。

### 在线编译

[online版本](http://remix.ethereum.org/) 可能卡，可能慢

最好用最不好用的都是这个东西……好用是因为确实好用，不好用是因为科学上网都解决不了他慢的问题。所以最好下载本地版的。
```
#安装remix
cnpm install remix-ide -g

#导入有eth的账户，要用这个账户来部署合约呢
geth account import private.key

#本地启动geth 开启rpc和控制台
#这里起的是一个孤立节点，是为了不与其他节点同步数据
geth --nodiscover console --rpc --rpcapi 'web3,eth,debug' --rpcport 8545 --rpccorsdomain '*'

#另开一个控制台！另开一个控制台！另开一个控制台！
#启动remix，默认链接本地8545端口的geth的rpc（就是上面启动那个）
remix-ide
```
浏览器访问 http://127.0.0.1:8080 就能看到编译器了。虽然也是比较慢，但总算是能用了。

### 其他编译器
#### truffle
开发框架，不仅带编译功能，还带部署功能。然而东西略复杂。不推荐用。如果非要用。自行google

#### solc
从头到尾就没调通。天坑之一


## 编写智能合约
官方推荐的solidity
- github：https://github.com/ethereum/solidity/tree/v0.4.21
- 官方文档：http://solidity.readthedocs.io/en/v0.4.21/

solidity更新了好几版了。学起来不算太难。不过**以发币为目的就不需要学太深入了**。直接用ERC20官方模板就可以。
- 官网：http://eips.ethereum.org/EIPS/eip-20
- github：https://github.com/ConsenSys/Tokens

**合约就在**https://github.com/ConsenSys/Tokens/tree/master/contracts/eip20

## 编译
浏览器访问 http://127.0.0.1:8080 打开编译器。

左上角有个文件夹的标识，打开到下载的目录。选中三个文件一起打开。

点击右上角`start to compile`

可能会报一些**黄色的warning** 不需要理会，只要不是**红色error**就可以。

然后在`start to compile`下面的选择框中选中`EIP20`，然后点击旁边的`Details`按钮(可能要等几秒钟)。

然后弹出框展示的就是部署需要的数据。需要的是`BYTECODE`和`ABI`。而`WEB3DEPLOY`将`BYTECODE`和`ABI`包装成了js脚本，可以直接在geth控制台执行来部署合约。只需要稍加修改就可以使用。

接下来开始部署。

## 部署
部署合约是要消耗eth的，消耗的eth被称为gas，又叫汽油，手续费。部署由矿工来搞定，gas就是要支付的酬劳。

部署合约的操作要转换成多个step，每个step的执行都要消耗gas。最终消耗的eth量由两个参数来控制：`gasPrice`和`gas`。前者表示每个step消耗的eth，后者表示限定`step`的最多执行数。最终消耗的eth是 `step * gas`。

本文部署`gasPrice`设定的是`2GWei`，略慢，可设定到4GWei。

但是`gas`的值一定要足够大，因为`gas`限定了执行step数，**如果step执行完而合约没部署完，那么操作回滚，gas不退。**

这是第一个坑，我第一次设置的gas为50000，结果gas清零，部署失败！因此gas不妨设置得尽可能大。之所以说尽肯能，是因为gas的上限受限于两个条件：1.以太坊规定的上限，2.持有的eth量

gas设置的前提是要求账户有足够的eth，eth保有量必须大于设置的`gas*gasPrice`。用不完的gas会退回来。这就意味着部署系统的账户，**必须能用`eth.getBalance()`查到余额，并且余额大于`gas*gasPrice`**

> 假设由`eth.accounts[0]`来部署，我们称为**部署账户**

部署分两步：
1. 加入变量，生成部署的操作码
2. 支付gas，将操作码广播

> 不仅是部署，任何在以太坊网络的操作都分两步：生成操作码，广播操作码。

部署有两种方式：
1. 在以太坊节点，执行上面复制的`WEB3DEPLOY`代码
2. 在以太坊节点获取部署的操作码，用metamask广播部署

第1种方法要求节点必须能查到`eth.accounts[0]`的余额。因为广播时会检查广播人的余额。如果查不到是无法广播的。**只有节点数据同步到一定程度才能查到余额**。所谓的一定程度是指：账户所在的数据块被同步。一般来说这是个短时间内无法完成的事情。

所以只能看**第2个部署的方法**

### 生成操作码
`WEB3DEPLOY`中是直接部署的语句，需要修改成获取操作码的语句。先看下`WEB3DEPLOY`的代码
```
var _initialAmount = /* var of type uint256 here */ ; //要发的token的数量
var _tokenName = /* var of type string here */ ; //token的名字
var _decimalUnits = /* var of type uint8 here */ ; //token精度
var _tokenSymbol = /* var of type string here */ ; //token的symbol
var eip20Contract = web3.eth.contract([{...}]); //contract括号内是abi，此处有省略
var eip20 = eip20Contract.new(                  //部署语句
   _initialAmount,
   _tokenName,
   _decimalUnits,
   _tokenSymbol,
   {
     from: web3.eth.accounts[0], 
     data: '0x6060604xxxxxxb0029',  //bytecode，此处有省略
     gas: '4700000'
   }, function (e, contract){
    console.log(e, contract);
    if (typeof contract.address !== 'undefined') {
         console.log('Contract mined! address: ' + contract.address + ' transactionHash: ' + contract.transactionHash);
    }
 })
```
在上面我们已经把部署账户的私钥添加进来，就用这个账户来生成操作码。然后在上面打开的geth终端中执行下面命令。

> 这里要注意精度。假设发行100万个token，最小单位是0.01。那么精度就是100，token的总量应该是 100万*100，即 100000000。

```
//从上面获取abi（第5行）和bytecode（第12行）赋值到下面两个变量
> var abi = [{.....}]
> var bytecode = '0x6060604xxxxxxb0029'
> var c = eth.contract(abi)
> var cData = {from:eth.accounts[0], data:bytecode} //部署账户
> var dcode = c.new.getData('100000000', 'myEth', 100, 'METH', cData)
```
执行过后上面的dcode就是要广播的操作码了。

### metamask部署
chrome安装metamask插件后，导入账户。然后点击send。
- Recipient Address 留空
- amount 填0
- transaction Data中填上操作码。点击next
- GasLimit: 5000000 GasPrice:3Gwei 点击submit 等待成功

部署需要一段时间。部署完成后点击部署成功的合约，可以跳到etherscan.io查看区块。这里要拿到合约地址。

### 查看token
还是因为数据同步的问题，只能在metamask中查看。

buy和send按钮下面有个token。点开后选择add token，填上获取到的合约地址，就能查看token余额了。

## 与合约交互
交互分读（查余额）和写（转账）。读不需要广播，不需要消耗gas，但转账属于写需要广播，就要消耗gas。

这里的转账本质是发消息，同样由于数据同步的问题，只能在geth控制台生成广播操作码，在metamask广播。

> 这里有个天坑，操作前一定要设置默认账户`eth.defaultAccount`否则转账操会一直提示`invalid address` 设置的defaultAcount是谁，操作就由谁来执行。

与合约交互，无论读写，都需要获取到合约的实例。获取合约实例需要abi和合约地址。以下还是geth中操作。
```
> eth.defaultAccount = eth.accounts[0]
> abi = [{.....}]
> add = '0xaaaaaaaa'
> cInstance = eth.contract(abi).at(add) //cInstance即为合约实例
> cInstance.balanceOf(eth.accounts[0]) //查询余额。结果不显示小数点，所以应该是1个亿
> cInstance.transfer.getData(eth.accounts[1], 10000) //向另一个地址转100，获取操作码 到metamask send
```

### 其他交互
所有的交互都是调用合约里定义的函数。如果有同步数据的前提，就只能先获取操作码，然后在metamask上广播。
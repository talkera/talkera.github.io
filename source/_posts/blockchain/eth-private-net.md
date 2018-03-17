---
title: 搭建以太坊私链和私链集群
date: 2018-03-16
categories: 区块链
tags: 
	- 区块链
	- 以太坊
---

开发以太坊相关还是私链更方便。

<!--more-->

[下载Geth](https://geth.ethereum.org/downloads/)

## 准备
创建一个为私链专用的系统用户：ethPrivate 在ethPrivate的home目录操作。

步骤：
1. 准备一个创世块文件`genesis.json`
2. 用`genesis.json`初始化数据到一个目录 `node1`
3. 在`node1`中启动一个节点
4. 在另一个目录`node2`，重复2操作，来为另一个节点做初始化
5. 在`node2`中启动另一个节点（端口号要区分开来）
6. 重复4,5步骤来启动下一个节点
7. 在启动的节点中添加其他节点


## 创世块文件
创世块文件用来初始化数据，如果私链上的节点要互联必须用同一个创世块文件来初始化。

初始化之后，启动节点时不会再读取该文件。

文件示例如下：`/home/ethPrivate/genesis.json`
```
{
	"config": {
			"chainId": 200,
			"homesteadBlock": 0,
			"eip155Block": 0,
			"eip158Block": 0
	},
	"coinbase" : "0x0000000000000000000000000000000000000000",
	"difficulty" : "0xffffff",
	"extraData" : "0x11bbe8db4e347b4e8c937c1c8370e4b5ed33adb3db69cbdb7a38e1e50b1b82fb",
	"gasLimit" : "0xffffffff",
	"nonce" : "0x0000000000000000",
	"mixhash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
	"parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
	"timestamp" : "0x00",
	"alloc": {
		"de1e758511a7c67e7db93d1c23c1060a21db4615":{"balance":"300000"},
		"27dc8de9e9a1cb673543bd5fce89e83af09e228f":{"balance":"500000"}
	}
}
```

key|说明
---|---
alloc | 用来预置账号以及账号的以太币数量。因为私有链挖矿比较容易，所以不需要预置有币的账号。
nonce | 生成的工作证明的散列。注意它和 mixhash 的设置需要满足以太坊的 Yellow paper,4.3.4.Block Header Validity,（44）章节所描述的条件。
difficulty | 设置当前区块的难度，如果难度过大，CPU 挖矿就越难，这里设置较小难度，方便 CPU 挖矿，十六进制。
mixhash | 与 nonce 配合用于挖矿，由上一个区块的一部分生成的 hash。注意它和 nonce 的设置需要满足以太坊的 Yellow paper,4.3.4.Block Header Validity,（44）章节所描述的条件。
coinbase | 矿工的账号，可以在创建私链之前导入已经创建好的账号。
parentHash | 上一个区块的 hash 值，创始区块没有上一个区块，所以设置为 0。
extraData | 附加信息，可以填写任意信息，已达到私有链的个性化，可以加入自己的名字等。
gasLimit | 该值设置对 GAS 的消耗总量限制，用来限制区块能包含的交易信息总和，私有链可以设置为最大值。


> 如果创始块文件或者网络id不一样，节点无法互联

### 挖矿难度
初始块的难度值决定整个链的速度. 
difficulty值 | 十进制 | 生成速度
--|--|--
2ffffd | 3145725 | 一分钟五到十个之间个
5ffffa | 6291450 | 一分钟五个
bffff4 | 12582900 | 六分钟挖了八个
3bfffc4| 62914500 | 六分钟两个
77fff88| 125829000 | 十分钟两个
12bffed4 | 314572500 | 15分钟没有一个

## 搭建网络
### 初始化和启动
```bash
#初始化两个节点
geth --datadir /home/ethPrivate/node0 init genesis.json
geth --datadir /home/ethPrivate/node1 init genesis.json

#后台启动第一个节点，网络id200，端口2001
nohup geth --datadir /home/ethPrivate/node0 --networkid 200 --port 2001 --nodiscover > /dev/null 2>&1 &
#后台启动第二个节点，网络id200，端口2011，rpc端口2012
nohup geth --datadir /home/ethPrivate/node1 --networkid 200 --port 2011 --nodiscover > /dev/null 2>&1 &
```

### 添加节点
节点启动后可以通过ipc，rpc，websocket等方式与节点通信。在同一台机器上一般使用geth 的ipc链接方式。不在同一台机器则使用其他方式。

ipc链接后对节点的操作不受限制。其他方式则需要单独开启，并且可能需要对各种模块的开放访问限制。

```
#链接到第1个节点
geth attach ipc:/home/ethPrivate/node0/geth.ipc

#查看节点信息
> admin.nodeInfo.enode
"enode://07007...cf6fa@[::]:2001?discport=0"
> exit

#链接到第2个节点
geth attach ipc:/home/ethPrivate/node1/geth.ipc

#查看伙伴节点
> admin.peers
[]
#添加伙伴节点
> admin.addPeer('enode://07007...cf6fa@[::]:2001?discport=0')
true
#再次查看伙伴节点
> admin.peers

[{
    caps: ["eth/63"],
    id: "07007...cf6fa",
    name: "Geth/v1.7.3-stable-4bb3c89d/linux-amd64/go1.9.2",
    network: {
      localAddress: "[::1]:37098",
      remoteAddress: "[::1]:2001"
    },
    protocols: {
      eth: {
        difficulty: 6291450,
        head: "0x3358e132db6a144c73f09605055def2720dc954983aa40c046ba8ca735e9e9d2",
        version: 63
      }
    }
}]
> exit
```
私链网络搭建完成。


## 自动化启动脚本

#### 初始化脚本
```
#!/bin/bash
node=$(( $1 + 0 ))
owner=ethPrivate
geth=/usr/local/bin/geth
init_file=/home/ethPrivate/genesis.json
netid=200
node_path=/home/ethPrivate/node${node}

if [ $EUID == 0 ];then
	su ${owner} -
fi

if [ `whoami` != ${owner} ];then
	echo "erro user"
	exit 0
fi

if [ -d $node_path ]
then
	echo "${node_path} exists."
	exit 0
else
	mkdir ${node_path}
fi

cat > $init_file <<FILEDATA
{
	"config": {
			"chainId": ${netid},
			"homesteadBlock": 0,
			"eip155Block": 0,
			"eip158Block": 0
	},
	"coinbase" : "0x0000000000000000000000000000000000000000",
	"difficulty" : "0x5ffffa",
	"extraData" : "0x11bbe8db4e347b4e8c937c1c8370e4b5ed33adb3db69cbdb7a38e1e50b1b82fb",
	"gasLimit" : "0xffffffff",
	"nonce" : "0x0000000000000000",
	"mixhash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
	"parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
	"timestamp" : "0x00",
	"alloc": {}
}
FILEDATA

cmd="${geth} --datadir ${node_path} init ${init_file}"
echo $cmd
$cmd
```

#### 启动节点脚本

```
#!/bin/bash
geth=/usr/local/bin/geth

node=$(( $1 + 0 ))
node_path=/home/ethPrivate/node${node}
if [ ! -d $node_path ];then
	echo "node path error:${node_path}"
	exit 0
fi

netid=200
port=$((2000 + $node * 10))
rpc_port=$((2001 + $node * 10))
websocket_port=$((2002 + $node * 10))
identity="${netid}_${port}"
ip_list="192.168.1.80"
log=${node_path}/nohup.log

if [ `ps -fe | grep geth | grep ${port} | grep -v 'grep' | wc -l` -gt 0 ];then
	echo "node is running"
	exit 0
fi

cmd="
nohup ${geth} \
--datadir ${node_path} \
--identity ${identity} \
--networkid ${netid} --port ${port} \
--nodiscover --maxpeers 3 \
--wsport ${websocket_port} \
--wsaddr ${ip_list} \
--wsapi 'db,eth,net,web3' \
--rpc --rpccorsdomain '*.anxindeli.com' \
--rpcport ${rpc_port} \
--rpcaddr ${ip_list} \
--rpcapi 'db,eth,net,web3,personal,admin' > ${log} 2>&1 &
"

echo $cmd

echo "connect cmd: geth attach ipc:${node_path}/geth.ipc"
```
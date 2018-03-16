---
title: ���̫��˽����˽����Ⱥ
date: 2018-03-16
categories: ������
tags: [������, ��̫��]
---

[����Geth](https://geth.ethereum.org/downloads/)

## ׼��
����һ��Ϊ˽��ר�õ�ϵͳ�û���ethPrivate ��ethPrivate��homeĿ¼������

���裺
1. ׼��һ���������ļ�`genesis.json`
2. ��`genesis.json`��ʼ�����ݵ�һ��Ŀ¼ `node1`
3. ��`node1`������һ���ڵ�
4. ����һ��Ŀ¼`node2`���ظ�2��������Ϊ��һ���ڵ�����ʼ��
5. ��`node2`��������һ���ڵ㣨�˿ں�Ҫ���ֿ�����
6. �ظ�4,5������������һ���ڵ�
7. �������Ľڵ�����������ڵ�


## �������ļ�
�������ļ�������ʼ�����ݣ����˽���ϵĽڵ�Ҫ����������ͬһ���������ļ�����ʼ����

��ʼ��֮�������ڵ�ʱ�����ٶ�ȡ���ļ���

�ļ�ʾ�����£�`/home/ethPrivate/genesis.json`
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

key|˵��
---|---
alloc | ����Ԥ���˺��Լ��˺ŵ���̫����������Ϊ˽�����ڿ�Ƚ����ף����Բ���ҪԤ���бҵ��˺š�
nonce | ���ɵĹ���֤����ɢ�С�ע������ mixhash ��������Ҫ������̫���� Yellow paper,4.3.4.Block Header Validity,��44���½���������������
difficulty | ���õ�ǰ������Ѷȣ�����Ѷȹ���CPU �ڿ��Խ�ѣ��������ý�С�Ѷȣ����� CPU �ڿ�ʮ�����ơ�
mixhash | �� nonce ��������ڿ�����һ�������һ�������ɵ� hash��ע������ nonce ��������Ҫ������̫���� Yellow paper,4.3.4.Block Header Validity,��44���½���������������
coinbase | �󹤵��˺ţ������ڴ���˽��֮ǰ�����Ѿ������õ��˺š�
parentHash | ��һ������� hash ֵ����ʼ����û����һ�����飬��������Ϊ 0��
extraData | ������Ϣ��������д������Ϣ���Ѵﵽ˽�����ĸ��Ի������Լ����Լ������ֵȡ�
gasLimit | ��ֵ���ö� GAS �������������ƣ��������������ܰ����Ľ�����Ϣ�ܺͣ�˽������������Ϊ���ֵ��


> �����ʼ���ļ���������id��һ�����ڵ��޷�����

### �ڿ��Ѷ�
��ʼ����Ѷ�ֵ�������������ٶ�. 
difficultyֵ | ʮ���� | �����ٶ�
--|--|--
2ffffd | 3145725 | һ�����嵽ʮ��֮���
5ffffa | 6291450 | һ�������
bffff4 | 12582900 | ���������˰˸�
3bfffc4| 62914500 | ����������
77fff88| 125829000 | ʮ��������
12bffed4 | 314572500 | 15����û��һ��

## �����
### ��ʼ��������
```bash
#��ʼ�������ڵ�
geth --datadir /home/ethPrivate/node0 init genesis.json
geth --datadir /home/ethPrivate/node1 init genesis.json

#��̨������һ���ڵ㣬����id200���˿�2001
nohup geth --datadir /home/ethPrivate/node0 --networkid 200 --port 2001 --nodiscover > /dev/null 2>&1 &
#��̨�����ڶ����ڵ㣬����id200���˿�2011��rpc�˿�2012
nohup geth --datadir /home/ethPrivate/node1 --networkid 200 --port 2011 --nodiscover > /dev/null 2>&1 &
```

### ��ӽڵ�
�ڵ����������ͨ��ipc��rpc��websocket�ȷ�ʽ��ڵ�ͨ�š���ͬһ̨������һ��ʹ��geth ��ipc���ӷ�ʽ������ͬһ̨������ʹ��������ʽ��

ipc���Ӻ�Խڵ�Ĳ����������ơ�������ʽ����Ҫ�������������ҿ�����Ҫ�Ը���ģ��Ŀ��ŷ������ơ�

```
#���ӵ���1���ڵ�
geth attach ipc:/home/ethPrivate/node0/geth.ipc

#�鿴�ڵ���Ϣ
> admin.nodeInfo.enode
"enode://07007...cf6fa@[::]:2001?discport=0"
> exit

#���ӵ���2���ڵ�
geth attach ipc:/home/ethPrivate/node1/geth.ipc

#�鿴���ڵ�
> admin.peers
[]
#��ӻ��ڵ�
> admin.addPeer('enode://07007...cf6fa@[::]:2001?discport=0')
true
#�ٴβ鿴���ڵ�
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
˽��������ɡ�


## �Զ��������ű�

#### ��ʼ���ű�
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

#### �����ڵ�ű�

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
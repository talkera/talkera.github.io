---
title: Geth控制台 - JavascriptAPI
date: 2018-03-16
categories: 区块链
tags: [区块链, 以太坊]
---

参考：https://ethereumbuilders.gitbooks.io/guide/content/en/ethereum_javascript_api.html

启动控制台有两种方式：
- 运行一个节点，并同时启动终端
- 用geth attach连接到一个**本地**的节点
```
#console表示启动JavaScript控制台
nohup geth --datadir /home/ethPrivate/node0 --networkid 200 --port 2001 --nodiscover console

#用geth连接本地一个节点
geth attach ipc:/home/ethPrivate/node1/geth.ipc
```



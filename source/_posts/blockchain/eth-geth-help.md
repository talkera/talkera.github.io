---
<<<<<<< HEAD
=======
<<<<<<< HEAD
>>>>>>> new articles
title: Geth�����ĵ�
date: 2018-03-16
categories: ������
tags: [������, ��̫��, Geth]
---

## ����
```
account    �����˻�
attach     ��������ʽJavaScript���������ӵ��ڵ㣩
bug        �ϱ�bug Issues
console    ��������ʽJavaScript����
copydb     ���ļ��д���������
dump       Dump��������һ���ض��Ŀ�洢
dumpconfig ��ʾ����ֵ
export     �������������ļ�
import     ����һ���������ļ�
init       ��������ʼ��һ���µĴ����Ϳ�
js         ִ��ָ����JavaScript�ļ�(���)
license    ��ʾ�����Ϣ
makecache  ����ethash��֤����(���ڲ���)
makedag    ����ethash �ڿ�DAG(���ڲ���)
monitor    ��غͿ��ӻ��ڵ�ָ��
removedb   ɾ����������״̬���ݿ�
version    ��ӡ�汾��
wallet     ����EthereumԤ��Ǯ��
help,h     ��ʾһ����������һ�������б�
```

## ��������
### ETHEREUMѡ��:
```
--config value          TOML �����ļ�
--datadir ��xxx��         ���ݿ��keystore��Կ������Ŀ¼
--keystore              keystore���Ŀ¼(Ĭ����datadir��)
--nousb                 ���ü�غ͹���USBӲ��Ǯ��
--networkid value       �����ʶ��(����, 1=Frontier, 2=Morden (����), 3=Ropsten, 4=Rinkeby) (Ĭ��: 1)
--testnet               Ropsten����:Ԥ�����õ�POW(proof-of-work)��������
--rinkeby               Rinkeby����: Ԥ�����õ�POA(proof-of-authority)��������
--syncmode "fast"       ͬ��ģʽ ("fast", "full", or "light")
--ethstats value        �ϱ�ethstats service  URL (nodename:secret@host:port)
--identity value        �Զ���ڵ���
--lightserv value       ����LES����ʱ�����ٷֱ�(0 �C 90)(Ĭ��ֵ:0) 
--lightpeers value      ���LES client peers����(Ĭ��ֵ:20)
--lightkdf              ��KDFǿ������ʱ����key-derivation RAM&CPUʹ��
```

### �����ߣ�ģʽ��ѡ��:
```
--dev               ʹ��POA��ʶ���磬Ĭ��Ԥ����һ���������˻����һ��Զ������ڿ�
--dev.period value  ������ģʽ���ڿ����� (0 = ���ڽ���ʱ) (Ĭ��: 0)
```

### ETHASH ѡ��:
```
--ethash.cachedir                        ethash��֤����Ŀ¼(Ĭ�� = datadirĿ¼��)
--ethash.cachesinmem value               ���ڴ汣��������ethash�������  (ÿ������16MB ) (Ĭ��: 2)
--ethash.cachesondisk value              �ڴ��̱���������ethash������� (ÿ������16MB) (Ĭ��: 3)
--ethash.dagdir ""                       ��ethash DAGsĿ¼ (Ĭ�� = �û�homĿ¼)
--ethash.dagsinmem value                 ���ڴ汣��������ethash DAGs ���� (ÿ��1GB����) (Ĭ��: 1)
--ethash.dagsondisk value                �ڴ��̱���������ethash DAGs ���� (ÿ��1GB����) (Ĭ��: 2)
```

### ���׳�ѡ��:
```
--txpool.nolocals            Ϊ�����ύ���׽��ü۸����
--txpool.journal value       ���ؽ��׵Ĵ�����־�����ڽڵ����� (Ĭ��: "transactions.rlp")
--txpool.rejournal value     �������ɱ��ؽ�����־��ʱ���� (Ĭ��: 1Сʱ)
--txpool.pricelimit value    ���뽻�׳ص���С��gas�۸�����(Ĭ��: 1)
--txpool.pricebump value     �۸񲨶��ٷֱȣ����֮ǰ���н��ף� (Ĭ��: 10)
--txpool.accountslots value  ÿ���ʻ���֤��ִ�е����ٽ��ײ�����  (Ĭ��: 16)
--txpool.globalslots value   �����ʻ���ִ�е�����ײ����� (Ĭ��: 4096)
--txpool.accountqueue value  ÿ���ʻ���������ǿ�ִ�н��ײ����� (Ĭ��: 64)
--txpool.globalqueue value   �����ʻ��ǿ�ִ�н�����������  (Ĭ��: 1024)
--txpool.lifetime value      �ǿ�ִ�н���������ʱ��(Ĭ��: 3Сʱ)
```
### ���ܵ��ŵ�ѡ��:
```
--cache value                ������ڲ�������ڴ�MB����������ֵ(���16 mb /���ݿ�ǿ��Ҫ��)(Ĭ��:128)
--trie-cache-gens value      �������ڴ��в�����trie node����(Ĭ��:120)
```
### �ʻ�ѡ��:
```
--unlock value              ������˻��ö��ŷָ�
--password value            ���ڷǽ���ʽ��������������ļ�
```
### API�Ϳ���̨ѡ��:
```
--rpc                       ����HTTP-RPC������
--rpcaddr value             HTTP-RPC�������ӿڵ�ַ(Ĭ��ֵ:��localhost��)
--rpcport value             HTTP-RPC�����������˿�(Ĭ��ֵ:8545)
--rpcapi value              ����HTTP-RPC�ӿ��ṩ��API
--ws                        ����WS-RPC������
--wsaddr value              WS-RPC�����������ӿڵ�ַ(Ĭ��ֵ:��localhost��)
--wsport value              WS-RPC�����������˿�(Ĭ��ֵ:8546)
--wsapi  value              ����WS-RPC�Ľӿ��ṩ��API
--wsorigins value           websockets���������Դ
--ipcdisable                ����IPC-RPC������
--ipcpath                   ������datadir���IPC socket/pipe�ļ���(ת�������ʽ·��)
--rpccorsdomain value       �����������������б�(���ŷָ�)(�����ǿ��)
--jspath loadScript         JavaScript���ؽű��ĸ�·��(Ĭ��ֵ:��.��)
--exec value                ִ��JavaScript���(ֻ�ܽ��console/attachʹ��)
--preload value             Ԥ���ص�����̨��JavaScript�ļ��б�(���ŷָ�)
```
### ����ѡ��:
```
--bootnodes value    ����P2P����������enode urls(���ŷָ�)(����light servers��v4+v5����)
--bootnodesv4 value  ����P2P v4����������enode urls(���ŷָ�) (light server, ȫ�ڵ�)
--bootnodesv5 value  ����P2P v5����������enode urls(���ŷָ�) (light server, ��ڵ�)
--port value         ���������˿�(Ĭ��ֵ:30303)
--maxpeers value     ��������ڵ�����(�������Ϊ0�����罫������)(Ĭ��ֵ:25)
--maxpendpeers value    ��������ӵ�����(�������Ϊ0����ʹ��Ĭ��ֵ)(Ĭ��ֵ:0)
--nat value             NAT�˿�ӳ����� (any|none|upnp|pmp|extip:<IP>) (Ĭ��: ��any��)
--nodiscover            ���ýڵ㷢�ֻ���(�ֶ���ӽڵ�)
--v5disc                ����ʵ���Ե�RLPx V5(Topic����)����
--nodekey value         P2P�ڵ���Կ�ļ�
--nodekeyhex value      ʮ�����Ƶ�P2P�ڵ���Կ(���ڲ���)
```

### ��ѡ��:
```
--mine                  ���ڿ�
--minerthreads value    �ڿ�ʹ�õ�CPU�߳�����(Ĭ��ֵ:8)
--etherbase value       �ڿ�����ַ(Ĭ��=��һ���������ʻ�)(Ĭ��ֵ:��0��)
--targetgaslimit value  Ŀ��gas���ƣ��������gas���ƣ�����������ᱻ�ڣ��� (Ĭ��ֵ:��4712388��)
--gasprice value        �ڿ���ܽ��׵����gas�۸�
--extradata value       �����õĶ��������(Ĭ��=client version)
```
### GAS�۸�ѡ��:
```
--gpoblocks value      ���ڼ��gas�۸�������ĸ���  (Ĭ��: 10)
--gpopercentile value  ����gas�۲ο�������׵�gas�۵İٷ�λ����(Ĭ��: 50)
```
### �������ѡ��:
```
--vmdebug        ��¼VM����Լ������Ϣ
```
### ��־�͵���ѡ��:
```
--metrics            ����metrics�ռ��ͱ���
--fakepow            ����proof-of-work��֤
--verbosity value    ��־��ϸ��:0=silent, 1=error, 2=warn, 3=info, 4=debug, 5=detail (default: 3)
--vmodule value      ÿ��ģ����ϸ��:�� <pattern>=<level>�Ķ��ŷָ��б� (���� eth/*=6,p2p=5)
--backtrace value    �����ض���־��¼��ջ���� (���� ��block.go:271��)
--debug                     ͻ����ʾ����λ����־(�ļ������к�)
--pprof                     ����pprof HTTP������
--pprofaddr value           pprof HTTP�����������ӿ�(Ĭ��ֵ:127.0.0.1)
--pprofport value           pprof HTTP�����������˿�(Ĭ��ֵ:6060)
--memprofilerate value      ��ָ��Ƶ�ʴ�memory profiling    (Ĭ��:524288)
--blockprofilerate value    ��ָ��Ƶ�ʴ�block profiling    (Ĭ��ֵ:0)
--cpuprofile value          ��CPU profileд��ָ���ļ�
--trace value               ��execution traceд��ָ���ļ�
```
### WHISPERʵ��ѡ��:
```
--shh                        ����Whisper
--shh.maxmessagesize value   �ɽ��ܵ�������Ϣ��С (Ĭ��ֵ: 1048576)
--shh.pow value              �ɽ��ܵ���С��POW (Ĭ��ֵ: 0.2)
```
### ����ѡ�
```
--fast     ��������ͬ��
--light    ������ͻ���ģʽ
<<<<<<< HEAD
=======
=======
title: Geth帮助文档
date: 2018-03-16
categories: 区块链
tags: [区块链, 以太坊, Geth]
---

## 命令
```
account    管理账户
attach     启动交互式JavaScript环境（连接到节点）
bug        上报bug Issues
console    启动交互式JavaScript环境
copydb     从文件夹创建本地链
dump       Dump（分析）一个特定的块存储
dumpconfig 显示配置值
export     导出区块链到文件
import     导入一个区块链文件
init       启动并初始化一个新的创世纪块
js         执行指定的JavaScript文件(多个)
license    显示许可信息
makecache  生成ethash验证缓存(用于测试)
makedag    生成ethash 挖矿DAG(用于测试)
monitor    监控和可视化节点指标
removedb   删除区块链和状态数据库
version    打印版本号
wallet     管理Ethereum预售钱包
help,h     显示一个命令或帮助一个命令列表
```

## 启动参数
### ETHEREUM选项:
```
--config value          TOML 配置文件
--datadir “xxx”         数据库和keystore密钥的数据目录
--keystore              keystore存放目录(默认在datadir内)
--nousb                 禁用监控和管理USB硬件钱包
--networkid value       网络标识符(整型, 1=Frontier, 2=Morden (弃用), 3=Ropsten, 4=Rinkeby) (默认: 1)
--testnet               Ropsten网络:预先配置的POW(proof-of-work)测试网络
--rinkeby               Rinkeby网络: 预先配置的POA(proof-of-authority)测试网络
--syncmode "fast"       同步模式 ("fast", "full", or "light")
--ethstats value        上报ethstats service  URL (nodename:secret@host:port)
--identity value        自定义节点名
--lightserv value       允许LES请求时间最大百分比(0 – 90)(默认值:0) 
--lightpeers value      最大LES client peers数量(默认值:20)
--lightkdf              在KDF强度消费时降低key-derivation RAM&CPU使用
```

### 开发者（模式）选项:
```
--dev               使用POA共识网络，默认预分配一个开发者账户并且会自动开启挖矿。
--dev.period value  开发者模式下挖矿周期 (0 = 仅在交易时) (默认: 0)
```

### ETHASH 选项:
```
--ethash.cachedir                        ethash验证缓存目录(默认 = datadir目录内)
--ethash.cachesinmem value               在内存保存的最近的ethash缓存个数  (每个缓存16MB ) (默认: 2)
--ethash.cachesondisk value              在磁盘保存的最近的ethash缓存个数 (每个缓存16MB) (默认: 3)
--ethash.dagdir ""                       存ethash DAGs目录 (默认 = 用户hom目录)
--ethash.dagsinmem value                 在内存保存的最近的ethash DAGs 个数 (每个1GB以上) (默认: 1)
--ethash.dagsondisk value                在磁盘保存的最近的ethash DAGs 个数 (每个1GB以上) (默认: 2)
```

### 交易池选项:
```
--txpool.nolocals            为本地提交交易禁用价格豁免
--txpool.journal value       本地交易的磁盘日志：用于节点重启 (默认: "transactions.rlp")
--txpool.rejournal value     重新生成本地交易日志的时间间隔 (默认: 1小时)
--txpool.pricelimit value    加入交易池的最小的gas价格限制(默认: 1)
--txpool.pricebump value     价格波动百分比（相对之前已有交易） (默认: 10)
--txpool.accountslots value  每个帐户保证可执行的最少交易槽数量  (默认: 16)
--txpool.globalslots value   所有帐户可执行的最大交易槽数量 (默认: 4096)
--txpool.accountqueue value  每个帐户允许的最多非可执行交易槽数量 (默认: 64)
--txpool.globalqueue value   所有帐户非可执行交易最大槽数量  (默认: 1024)
--txpool.lifetime value      非可执行交易最大入队时间(默认: 3小时)
```
### 性能调优的选项:
```
--cache value                分配给内部缓存的内存MB数量，缓存值(最低16 mb /数据库强制要求)(默认:128)
--trie-cache-gens value      保持在内存中产生的trie node数量(默认:120)
```
### 帐户选项:
```
--unlock value              需解锁账户用逗号分隔
--password value            用于非交互式密码输入的密码文件
```
### API和控制台选项:
```
--rpc                       启用HTTP-RPC服务器
--rpcaddr value             HTTP-RPC服务器接口地址(默认值:“localhost”)
--rpcport value             HTTP-RPC服务器监听端口(默认值:8545)
--rpcapi value              基于HTTP-RPC接口提供的API
--ws                        启用WS-RPC服务器
--wsaddr value              WS-RPC服务器监听接口地址(默认值:“localhost”)
--wsport value              WS-RPC服务器监听端口(默认值:8546)
--wsapi  value              基于WS-RPC的接口提供的API
--wsorigins value           websockets请求允许的源
--ipcdisable                禁用IPC-RPC服务器
--ipcpath                   包含在datadir里的IPC socket/pipe文件名(转义过的显式路径)
--rpccorsdomain value       允许跨域请求的域名列表(逗号分隔)(浏览器强制)
--jspath loadScript         JavaScript加载脚本的根路径(默认值:“.”)
--exec value                执行JavaScript语句(只能结合console/attach使用)
--preload value             预加载到控制台的JavaScript文件列表(逗号分隔)
```
### 网络选项:
```
--bootnodes value    用于P2P发现引导的enode urls(逗号分隔)(对于light servers用v4+v5代替)
--bootnodesv4 value  用于P2P v4发现引导的enode urls(逗号分隔) (light server, 全节点)
--bootnodesv5 value  用于P2P v5发现引导的enode urls(逗号分隔) (light server, 轻节点)
--port value         网卡监听端口(默认值:30303)
--maxpeers value     最大的网络节点数量(如果设置为0，网络将被禁用)(默认值:25)
--maxpendpeers value    最大尝试连接的数量(如果设置为0，则将使用默认值)(默认值:0)
--nat value             NAT端口映射机制 (any|none|upnp|pmp|extip:<IP>) (默认: “any”)
--nodiscover            禁用节点发现机制(手动添加节点)
--v5disc                启用实验性的RLPx V5(Topic发现)机制
--nodekey value         P2P节点密钥文件
--nodekeyhex value      十六进制的P2P节点密钥(用于测试)
```

### 矿工选项:
```
--mine                  打开挖矿
--minerthreads value    挖矿使用的CPU线程数量(默认值:8)
--etherbase value       挖矿奖励地址(默认=第一个创建的帐户)(默认值:“0”)
--targetgaslimit value  目标gas限制：设置最低gas限制（低于这个不会被挖？） (默认值:“4712388”)
--gasprice value        挖矿接受交易的最低gas价格
--extradata value       矿工设置的额外块数据(默认=client version)
```
### GAS价格选项:
```
--gpoblocks value      用于检查gas价格的最近块的个数  (默认: 10)
--gpopercentile value  建议gas价参考最近交易的gas价的百分位数，(默认: 50)
```
### 虚拟机的选项:
```
--vmdebug        记录VM及合约调试信息
```
### 日志和调试选项:
```
--metrics            启用metrics收集和报告
--fakepow            禁用proof-of-work验证
--verbosity value    日志详细度:0=silent, 1=error, 2=warn, 3=info, 4=debug, 5=detail (default: 3)
--vmodule value      每个模块详细度:以 <pattern>=<level>的逗号分隔列表 (比如 eth/*=6,p2p=5)
--backtrace value    请求特定日志记录堆栈跟踪 (比如 “block.go:271”)
--debug                     突出显示调用位置日志(文件名及行号)
--pprof                     启用pprof HTTP服务器
--pprofaddr value           pprof HTTP服务器监听接口(默认值:127.0.0.1)
--pprofport value           pprof HTTP服务器监听端口(默认值:6060)
--memprofilerate value      按指定频率打开memory profiling    (默认:524288)
--blockprofilerate value    按指定频率打开block profiling    (默认值:0)
--cpuprofile value          将CPU profile写入指定文件
--trace value               将execution trace写入指定文件
```
### WHISPER实验选项:
```
--shh                        启用Whisper
--shh.maxmessagesize value   可接受的最大的消息大小 (默认值: 1048576)
--shh.pow value              可接受的最小的POW (默认值: 0.2)
```
### 弃用选项：
```
--fast     开启快速同步
--light    启用轻客户端模式
>>>>>>> new articles
>>>>>>> new articles
```
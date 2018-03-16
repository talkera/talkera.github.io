---
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
```
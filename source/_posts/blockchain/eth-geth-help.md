---
<<<<<<< HEAD
=======
<<<<<<< HEAD
>>>>>>> new articles
title: Geth°ïÖúÎÄµµ
date: 2018-03-16
categories: Çø¿éÁ´
tags: [Çø¿éÁ´, ÒÔÌ«·», Geth]
---

## ÃüÁî
```
account    ¹ÜÀíÕË»§
attach     Æô¶¯½»»¥Ê½JavaScript»·¾³£¨Á¬½Óµ½½Úµã£©
bug        ÉÏ±¨bug Issues
console    Æô¶¯½»»¥Ê½JavaScript»·¾³
copydb     ´ÓÎÄ¼ş¼Ğ´´½¨±¾µØÁ´
dump       Dump£¨·ÖÎö£©Ò»¸öÌØ¶¨µÄ¿é´æ´¢
dumpconfig ÏÔÊ¾ÅäÖÃÖµ
export     µ¼³öÇø¿éÁ´µ½ÎÄ¼ş
import     µ¼ÈëÒ»¸öÇø¿éÁ´ÎÄ¼ş
init       Æô¶¯²¢³õÊ¼»¯Ò»¸öĞÂµÄ´´ÊÀ¼Í¿é
js         Ö´ĞĞÖ¸¶¨µÄJavaScriptÎÄ¼ş(¶à¸ö)
license    ÏÔÊ¾Ğí¿ÉĞÅÏ¢
makecache  Éú³ÉethashÑéÖ¤»º´æ(ÓÃÓÚ²âÊÔ)
makedag    Éú³Éethash ÍÚ¿óDAG(ÓÃÓÚ²âÊÔ)
monitor    ¼à¿ØºÍ¿ÉÊÓ»¯½ÚµãÖ¸±ê
removedb   É¾³ıÇø¿éÁ´ºÍ×´Ì¬Êı¾İ¿â
version    ´òÓ¡°æ±¾ºÅ
wallet     ¹ÜÀíEthereumÔ¤ÊÛÇ®°ü
help,h     ÏÔÊ¾Ò»¸öÃüÁî»ò°ïÖúÒ»¸öÃüÁîÁĞ±í
```

## Æô¶¯²ÎÊı
### ETHEREUMÑ¡Ïî:
```
--config value          TOML ÅäÖÃÎÄ¼ş
--datadir ¡°xxx¡±         Êı¾İ¿âºÍkeystoreÃÜÔ¿µÄÊı¾İÄ¿Â¼
--keystore              keystore´æ·ÅÄ¿Â¼(Ä¬ÈÏÔÚdatadirÄÚ)
--nousb                 ½ûÓÃ¼à¿ØºÍ¹ÜÀíUSBÓ²¼şÇ®°ü
--networkid value       ÍøÂç±êÊ¶·û(ÕûĞÍ, 1=Frontier, 2=Morden (ÆúÓÃ), 3=Ropsten, 4=Rinkeby) (Ä¬ÈÏ: 1)
--testnet               RopstenÍøÂç:Ô¤ÏÈÅäÖÃµÄPOW(proof-of-work)²âÊÔÍøÂç
--rinkeby               RinkebyÍøÂç: Ô¤ÏÈÅäÖÃµÄPOA(proof-of-authority)²âÊÔÍøÂç
--syncmode "fast"       Í¬²½Ä£Ê½ ("fast", "full", or "light")
--ethstats value        ÉÏ±¨ethstats service  URL (nodename:secret@host:port)
--identity value        ×Ô¶¨Òå½ÚµãÃû
--lightserv value       ÔÊĞíLESÇëÇóÊ±¼ä×î´ó°Ù·Ö±È(0 ¨C 90)(Ä¬ÈÏÖµ:0) 
--lightpeers value      ×î´óLES client peersÊıÁ¿(Ä¬ÈÏÖµ:20)
--lightkdf              ÔÚKDFÇ¿¶ÈÏû·ÑÊ±½µµÍkey-derivation RAM&CPUÊ¹ÓÃ
```

### ¿ª·¢Õß£¨Ä£Ê½£©Ñ¡Ïî:
```
--dev               Ê¹ÓÃPOA¹²Ê¶ÍøÂç£¬Ä¬ÈÏÔ¤·ÖÅäÒ»¸ö¿ª·¢ÕßÕË»§²¢ÇÒ»á×Ô¶¯¿ªÆôÍÚ¿ó¡£
--dev.period value  ¿ª·¢ÕßÄ£Ê½ÏÂÍÚ¿óÖÜÆÚ (0 = ½öÔÚ½»Ò×Ê±) (Ä¬ÈÏ: 0)
```

### ETHASH Ñ¡Ïî:
```
--ethash.cachedir                        ethashÑéÖ¤»º´æÄ¿Â¼(Ä¬ÈÏ = datadirÄ¿Â¼ÄÚ)
--ethash.cachesinmem value               ÔÚÄÚ´æ±£´æµÄ×î½üµÄethash»º´æ¸öÊı  (Ã¿¸ö»º´æ16MB ) (Ä¬ÈÏ: 2)
--ethash.cachesondisk value              ÔÚ´ÅÅÌ±£´æµÄ×î½üµÄethash»º´æ¸öÊı (Ã¿¸ö»º´æ16MB) (Ä¬ÈÏ: 3)
--ethash.dagdir ""                       ´æethash DAGsÄ¿Â¼ (Ä¬ÈÏ = ÓÃ»§homÄ¿Â¼)
--ethash.dagsinmem value                 ÔÚÄÚ´æ±£´æµÄ×î½üµÄethash DAGs ¸öÊı (Ã¿¸ö1GBÒÔÉÏ) (Ä¬ÈÏ: 1)
--ethash.dagsondisk value                ÔÚ´ÅÅÌ±£´æµÄ×î½üµÄethash DAGs ¸öÊı (Ã¿¸ö1GBÒÔÉÏ) (Ä¬ÈÏ: 2)
```

### ½»Ò×³ØÑ¡Ïî:
```
--txpool.nolocals            Îª±¾µØÌá½»½»Ò×½ûÓÃ¼Û¸ñ»íÃâ
--txpool.journal value       ±¾µØ½»Ò×µÄ´ÅÅÌÈÕÖ¾£ºÓÃÓÚ½ÚµãÖØÆô (Ä¬ÈÏ: "transactions.rlp")
--txpool.rejournal value     ÖØĞÂÉú³É±¾µØ½»Ò×ÈÕÖ¾µÄÊ±¼ä¼ä¸ô (Ä¬ÈÏ: 1Ğ¡Ê±)
--txpool.pricelimit value    ¼ÓÈë½»Ò×³ØµÄ×îĞ¡µÄgas¼Û¸ñÏŞÖÆ(Ä¬ÈÏ: 1)
--txpool.pricebump value     ¼Û¸ñ²¨¶¯°Ù·Ö±È£¨Ïà¶ÔÖ®Ç°ÒÑÓĞ½»Ò×£© (Ä¬ÈÏ: 10)
--txpool.accountslots value  Ã¿¸öÕÊ»§±£Ö¤¿ÉÖ´ĞĞµÄ×îÉÙ½»Ò×²ÛÊıÁ¿  (Ä¬ÈÏ: 16)
--txpool.globalslots value   ËùÓĞÕÊ»§¿ÉÖ´ĞĞµÄ×î´ó½»Ò×²ÛÊıÁ¿ (Ä¬ÈÏ: 4096)
--txpool.accountqueue value  Ã¿¸öÕÊ»§ÔÊĞíµÄ×î¶à·Ç¿ÉÖ´ĞĞ½»Ò×²ÛÊıÁ¿ (Ä¬ÈÏ: 64)
--txpool.globalqueue value   ËùÓĞÕÊ»§·Ç¿ÉÖ´ĞĞ½»Ò××î´ó²ÛÊıÁ¿  (Ä¬ÈÏ: 1024)
--txpool.lifetime value      ·Ç¿ÉÖ´ĞĞ½»Ò××î´óÈë¶ÓÊ±¼ä(Ä¬ÈÏ: 3Ğ¡Ê±)
```
### ĞÔÄÜµ÷ÓÅµÄÑ¡Ïî:
```
--cache value                ·ÖÅä¸øÄÚ²¿»º´æµÄÄÚ´æMBÊıÁ¿£¬»º´æÖµ(×îµÍ16 mb /Êı¾İ¿âÇ¿ÖÆÒªÇó)(Ä¬ÈÏ:128)
--trie-cache-gens value      ±£³ÖÔÚÄÚ´æÖĞ²úÉúµÄtrie nodeÊıÁ¿(Ä¬ÈÏ:120)
```
### ÕÊ»§Ñ¡Ïî:
```
--unlock value              Ğè½âËøÕË»§ÓÃ¶ººÅ·Ö¸ô
--password value            ÓÃÓÚ·Ç½»»¥Ê½ÃÜÂëÊäÈëµÄÃÜÂëÎÄ¼ş
```
### APIºÍ¿ØÖÆÌ¨Ñ¡Ïî:
```
--rpc                       ÆôÓÃHTTP-RPC·şÎñÆ÷
--rpcaddr value             HTTP-RPC·şÎñÆ÷½Ó¿ÚµØÖ·(Ä¬ÈÏÖµ:¡°localhost¡±)
--rpcport value             HTTP-RPC·şÎñÆ÷¼àÌı¶Ë¿Ú(Ä¬ÈÏÖµ:8545)
--rpcapi value              »ùÓÚHTTP-RPC½Ó¿ÚÌá¹©µÄAPI
--ws                        ÆôÓÃWS-RPC·şÎñÆ÷
--wsaddr value              WS-RPC·şÎñÆ÷¼àÌı½Ó¿ÚµØÖ·(Ä¬ÈÏÖµ:¡°localhost¡±)
--wsport value              WS-RPC·şÎñÆ÷¼àÌı¶Ë¿Ú(Ä¬ÈÏÖµ:8546)
--wsapi  value              »ùÓÚWS-RPCµÄ½Ó¿ÚÌá¹©µÄAPI
--wsorigins value           websocketsÇëÇóÔÊĞíµÄÔ´
--ipcdisable                ½ûÓÃIPC-RPC·şÎñÆ÷
--ipcpath                   °üº¬ÔÚdatadirÀïµÄIPC socket/pipeÎÄ¼şÃû(×ªÒå¹ıµÄÏÔÊ½Â·¾¶)
--rpccorsdomain value       ÔÊĞí¿çÓòÇëÇóµÄÓòÃûÁĞ±í(¶ººÅ·Ö¸ô)(ä¯ÀÀÆ÷Ç¿ÖÆ)
--jspath loadScript         JavaScript¼ÓÔØ½Å±¾µÄ¸ùÂ·¾¶(Ä¬ÈÏÖµ:¡°.¡±)
--exec value                Ö´ĞĞJavaScriptÓï¾ä(Ö»ÄÜ½áºÏconsole/attachÊ¹ÓÃ)
--preload value             Ô¤¼ÓÔØµ½¿ØÖÆÌ¨µÄJavaScriptÎÄ¼şÁĞ±í(¶ººÅ·Ö¸ô)
```
### ÍøÂçÑ¡Ïî:
```
--bootnodes value    ÓÃÓÚP2P·¢ÏÖÒıµ¼µÄenode urls(¶ººÅ·Ö¸ô)(¶ÔÓÚlight serversÓÃv4+v5´úÌæ)
--bootnodesv4 value  ÓÃÓÚP2P v4·¢ÏÖÒıµ¼µÄenode urls(¶ººÅ·Ö¸ô) (light server, È«½Úµã)
--bootnodesv5 value  ÓÃÓÚP2P v5·¢ÏÖÒıµ¼µÄenode urls(¶ººÅ·Ö¸ô) (light server, Çá½Úµã)
--port value         Íø¿¨¼àÌı¶Ë¿Ú(Ä¬ÈÏÖµ:30303)
--maxpeers value     ×î´óµÄÍøÂç½ÚµãÊıÁ¿(Èç¹ûÉèÖÃÎª0£¬ÍøÂç½«±»½ûÓÃ)(Ä¬ÈÏÖµ:25)
--maxpendpeers value    ×î´ó³¢ÊÔÁ¬½ÓµÄÊıÁ¿(Èç¹ûÉèÖÃÎª0£¬Ôò½«Ê¹ÓÃÄ¬ÈÏÖµ)(Ä¬ÈÏÖµ:0)
--nat value             NAT¶Ë¿ÚÓ³Éä»úÖÆ (any|none|upnp|pmp|extip:<IP>) (Ä¬ÈÏ: ¡°any¡±)
--nodiscover            ½ûÓÃ½Úµã·¢ÏÖ»úÖÆ(ÊÖ¶¯Ìí¼Ó½Úµã)
--v5disc                ÆôÓÃÊµÑéĞÔµÄRLPx V5(Topic·¢ÏÖ)»úÖÆ
--nodekey value         P2P½ÚµãÃÜÔ¿ÎÄ¼ş
--nodekeyhex value      Ê®Áù½øÖÆµÄP2P½ÚµãÃÜÔ¿(ÓÃÓÚ²âÊÔ)
```

### ¿ó¹¤Ñ¡Ïî:
```
--mine                  ´ò¿ªÍÚ¿ó
--minerthreads value    ÍÚ¿óÊ¹ÓÃµÄCPUÏß³ÌÊıÁ¿(Ä¬ÈÏÖµ:8)
--etherbase value       ÍÚ¿ó½±ÀøµØÖ·(Ä¬ÈÏ=µÚÒ»¸ö´´½¨µÄÕÊ»§)(Ä¬ÈÏÖµ:¡°0¡±)
--targetgaslimit value  Ä¿±êgasÏŞÖÆ£ºÉèÖÃ×îµÍgasÏŞÖÆ£¨µÍÓÚÕâ¸ö²»»á±»ÍÚ£¿£© (Ä¬ÈÏÖµ:¡°4712388¡±)
--gasprice value        ÍÚ¿ó½ÓÊÜ½»Ò×µÄ×îµÍgas¼Û¸ñ
--extradata value       ¿ó¹¤ÉèÖÃµÄ¶îÍâ¿éÊı¾İ(Ä¬ÈÏ=client version)
```
### GAS¼Û¸ñÑ¡Ïî:
```
--gpoblocks value      ÓÃÓÚ¼ì²égas¼Û¸ñµÄ×î½ü¿éµÄ¸öÊı  (Ä¬ÈÏ: 10)
--gpopercentile value  ½¨Òégas¼Û²Î¿¼×î½ü½»Ò×µÄgas¼ÛµÄ°Ù·ÖÎ»Êı£¬(Ä¬ÈÏ: 50)
```
### ĞéÄâ»úµÄÑ¡Ïî:
```
--vmdebug        ¼ÇÂ¼VM¼°ºÏÔ¼µ÷ÊÔĞÅÏ¢
```
### ÈÕÖ¾ºÍµ÷ÊÔÑ¡Ïî:
```
--metrics            ÆôÓÃmetricsÊÕ¼¯ºÍ±¨¸æ
--fakepow            ½ûÓÃproof-of-workÑéÖ¤
--verbosity value    ÈÕÖ¾ÏêÏ¸¶È:0=silent, 1=error, 2=warn, 3=info, 4=debug, 5=detail (default: 3)
--vmodule value      Ã¿¸öÄ£¿éÏêÏ¸¶È:ÒÔ <pattern>=<level>µÄ¶ººÅ·Ö¸ôÁĞ±í (±ÈÈç eth/*=6,p2p=5)
--backtrace value    ÇëÇóÌØ¶¨ÈÕÖ¾¼ÇÂ¼¶ÑÕ»¸ú×Ù (±ÈÈç ¡°block.go:271¡±)
--debug                     Í»³öÏÔÊ¾µ÷ÓÃÎ»ÖÃÈÕÖ¾(ÎÄ¼şÃû¼°ĞĞºÅ)
--pprof                     ÆôÓÃpprof HTTP·şÎñÆ÷
--pprofaddr value           pprof HTTP·şÎñÆ÷¼àÌı½Ó¿Ú(Ä¬ÈÏÖµ:127.0.0.1)
--pprofport value           pprof HTTP·şÎñÆ÷¼àÌı¶Ë¿Ú(Ä¬ÈÏÖµ:6060)
--memprofilerate value      °´Ö¸¶¨ÆµÂÊ´ò¿ªmemory profiling    (Ä¬ÈÏ:524288)
--blockprofilerate value    °´Ö¸¶¨ÆµÂÊ´ò¿ªblock profiling    (Ä¬ÈÏÖµ:0)
--cpuprofile value          ½«CPU profileĞ´ÈëÖ¸¶¨ÎÄ¼ş
--trace value               ½«execution traceĞ´ÈëÖ¸¶¨ÎÄ¼ş
```
### WHISPERÊµÑéÑ¡Ïî:
```
--shh                        ÆôÓÃWhisper
--shh.maxmessagesize value   ¿É½ÓÊÜµÄ×î´óµÄÏûÏ¢´óĞ¡ (Ä¬ÈÏÖµ: 1048576)
--shh.pow value              ¿É½ÓÊÜµÄ×îĞ¡µÄPOW (Ä¬ÈÏÖµ: 0.2)
```
### ÆúÓÃÑ¡Ïî£º
```
--fast     ¿ªÆô¿ìËÙÍ¬²½
--light    ÆôÓÃÇá¿Í»§¶ËÄ£Ê½
<<<<<<< HEAD
=======
=======
title: Gethå¸®åŠ©æ–‡æ¡£
date: 2018-03-16
categories: åŒºå—é“¾
tags: [åŒºå—é“¾, ä»¥å¤ªåŠ, Geth]
---

## å‘½ä»¤
```
account    ç®¡ç†è´¦æˆ·
attach     å¯åŠ¨äº¤äº’å¼JavaScriptç¯å¢ƒï¼ˆè¿æ¥åˆ°èŠ‚ç‚¹ï¼‰
bug        ä¸ŠæŠ¥bug Issues
console    å¯åŠ¨äº¤äº’å¼JavaScriptç¯å¢ƒ
copydb     ä»æ–‡ä»¶å¤¹åˆ›å»ºæœ¬åœ°é“¾
dump       Dumpï¼ˆåˆ†æï¼‰ä¸€ä¸ªç‰¹å®šçš„å—å­˜å‚¨
dumpconfig æ˜¾ç¤ºé…ç½®å€¼
export     å¯¼å‡ºåŒºå—é“¾åˆ°æ–‡ä»¶
import     å¯¼å…¥ä¸€ä¸ªåŒºå—é“¾æ–‡ä»¶
init       å¯åŠ¨å¹¶åˆå§‹åŒ–ä¸€ä¸ªæ–°çš„åˆ›ä¸–çºªå—
js         æ‰§è¡ŒæŒ‡å®šçš„JavaScriptæ–‡ä»¶(å¤šä¸ª)
license    æ˜¾ç¤ºè®¸å¯ä¿¡æ¯
makecache  ç”ŸæˆethashéªŒè¯ç¼“å­˜(ç”¨äºæµ‹è¯•)
makedag    ç”Ÿæˆethash æŒ–çŸ¿DAG(ç”¨äºæµ‹è¯•)
monitor    ç›‘æ§å’Œå¯è§†åŒ–èŠ‚ç‚¹æŒ‡æ ‡
removedb   åˆ é™¤åŒºå—é“¾å’ŒçŠ¶æ€æ•°æ®åº“
version    æ‰“å°ç‰ˆæœ¬å·
wallet     ç®¡ç†Ethereumé¢„å”®é’±åŒ…
help,h     æ˜¾ç¤ºä¸€ä¸ªå‘½ä»¤æˆ–å¸®åŠ©ä¸€ä¸ªå‘½ä»¤åˆ—è¡¨
```

## å¯åŠ¨å‚æ•°
### ETHEREUMé€‰é¡¹:
```
--config value          TOML é…ç½®æ–‡ä»¶
--datadir â€œxxxâ€         æ•°æ®åº“å’Œkeystoreå¯†é’¥çš„æ•°æ®ç›®å½•
--keystore              keystoreå­˜æ”¾ç›®å½•(é»˜è®¤åœ¨datadirå†…)
--nousb                 ç¦ç”¨ç›‘æ§å’Œç®¡ç†USBç¡¬ä»¶é’±åŒ…
--networkid value       ç½‘ç»œæ ‡è¯†ç¬¦(æ•´å‹, 1=Frontier, 2=Morden (å¼ƒç”¨), 3=Ropsten, 4=Rinkeby) (é»˜è®¤: 1)
--testnet               Ropstenç½‘ç»œ:é¢„å…ˆé…ç½®çš„POW(proof-of-work)æµ‹è¯•ç½‘ç»œ
--rinkeby               Rinkebyç½‘ç»œ: é¢„å…ˆé…ç½®çš„POA(proof-of-authority)æµ‹è¯•ç½‘ç»œ
--syncmode "fast"       åŒæ­¥æ¨¡å¼ ("fast", "full", or "light")
--ethstats value        ä¸ŠæŠ¥ethstats service  URL (nodename:secret@host:port)
--identity value        è‡ªå®šä¹‰èŠ‚ç‚¹å
--lightserv value       å…è®¸LESè¯·æ±‚æ—¶é—´æœ€å¤§ç™¾åˆ†æ¯”(0 â€“ 90)(é»˜è®¤å€¼:0) 
--lightpeers value      æœ€å¤§LES client peersæ•°é‡(é»˜è®¤å€¼:20)
--lightkdf              åœ¨KDFå¼ºåº¦æ¶ˆè´¹æ—¶é™ä½key-derivation RAM&CPUä½¿ç”¨
```

### å¼€å‘è€…ï¼ˆæ¨¡å¼ï¼‰é€‰é¡¹:
```
--dev               ä½¿ç”¨POAå…±è¯†ç½‘ç»œï¼Œé»˜è®¤é¢„åˆ†é…ä¸€ä¸ªå¼€å‘è€…è´¦æˆ·å¹¶ä¸”ä¼šè‡ªåŠ¨å¼€å¯æŒ–çŸ¿ã€‚
--dev.period value  å¼€å‘è€…æ¨¡å¼ä¸‹æŒ–çŸ¿å‘¨æœŸ (0 = ä»…åœ¨äº¤æ˜“æ—¶) (é»˜è®¤: 0)
```

### ETHASH é€‰é¡¹:
```
--ethash.cachedir                        ethashéªŒè¯ç¼“å­˜ç›®å½•(é»˜è®¤ = datadirç›®å½•å†…)
--ethash.cachesinmem value               åœ¨å†…å­˜ä¿å­˜çš„æœ€è¿‘çš„ethashç¼“å­˜ä¸ªæ•°  (æ¯ä¸ªç¼“å­˜16MB ) (é»˜è®¤: 2)
--ethash.cachesondisk value              åœ¨ç£ç›˜ä¿å­˜çš„æœ€è¿‘çš„ethashç¼“å­˜ä¸ªæ•° (æ¯ä¸ªç¼“å­˜16MB) (é»˜è®¤: 3)
--ethash.dagdir ""                       å­˜ethash DAGsç›®å½• (é»˜è®¤ = ç”¨æˆ·homç›®å½•)
--ethash.dagsinmem value                 åœ¨å†…å­˜ä¿å­˜çš„æœ€è¿‘çš„ethash DAGs ä¸ªæ•° (æ¯ä¸ª1GBä»¥ä¸Š) (é»˜è®¤: 1)
--ethash.dagsondisk value                åœ¨ç£ç›˜ä¿å­˜çš„æœ€è¿‘çš„ethash DAGs ä¸ªæ•° (æ¯ä¸ª1GBä»¥ä¸Š) (é»˜è®¤: 2)
```

### äº¤æ˜“æ± é€‰é¡¹:
```
--txpool.nolocals            ä¸ºæœ¬åœ°æäº¤äº¤æ˜“ç¦ç”¨ä»·æ ¼è±å…
--txpool.journal value       æœ¬åœ°äº¤æ˜“çš„ç£ç›˜æ—¥å¿—ï¼šç”¨äºèŠ‚ç‚¹é‡å¯ (é»˜è®¤: "transactions.rlp")
--txpool.rejournal value     é‡æ–°ç”Ÿæˆæœ¬åœ°äº¤æ˜“æ—¥å¿—çš„æ—¶é—´é—´éš” (é»˜è®¤: 1å°æ—¶)
--txpool.pricelimit value    åŠ å…¥äº¤æ˜“æ± çš„æœ€å°çš„gasä»·æ ¼é™åˆ¶(é»˜è®¤: 1)
--txpool.pricebump value     ä»·æ ¼æ³¢åŠ¨ç™¾åˆ†æ¯”ï¼ˆç›¸å¯¹ä¹‹å‰å·²æœ‰äº¤æ˜“ï¼‰ (é»˜è®¤: 10)
--txpool.accountslots value  æ¯ä¸ªå¸æˆ·ä¿è¯å¯æ‰§è¡Œçš„æœ€å°‘äº¤æ˜“æ§½æ•°é‡  (é»˜è®¤: 16)
--txpool.globalslots value   æ‰€æœ‰å¸æˆ·å¯æ‰§è¡Œçš„æœ€å¤§äº¤æ˜“æ§½æ•°é‡ (é»˜è®¤: 4096)
--txpool.accountqueue value  æ¯ä¸ªå¸æˆ·å…è®¸çš„æœ€å¤šéå¯æ‰§è¡Œäº¤æ˜“æ§½æ•°é‡ (é»˜è®¤: 64)
--txpool.globalqueue value   æ‰€æœ‰å¸æˆ·éå¯æ‰§è¡Œäº¤æ˜“æœ€å¤§æ§½æ•°é‡  (é»˜è®¤: 1024)
--txpool.lifetime value      éå¯æ‰§è¡Œäº¤æ˜“æœ€å¤§å…¥é˜Ÿæ—¶é—´(é»˜è®¤: 3å°æ—¶)
```
### æ€§èƒ½è°ƒä¼˜çš„é€‰é¡¹:
```
--cache value                åˆ†é…ç»™å†…éƒ¨ç¼“å­˜çš„å†…å­˜MBæ•°é‡ï¼Œç¼“å­˜å€¼(æœ€ä½16 mb /æ•°æ®åº“å¼ºåˆ¶è¦æ±‚)(é»˜è®¤:128)
--trie-cache-gens value      ä¿æŒåœ¨å†…å­˜ä¸­äº§ç”Ÿçš„trie nodeæ•°é‡(é»˜è®¤:120)
```
### å¸æˆ·é€‰é¡¹:
```
--unlock value              éœ€è§£é”è´¦æˆ·ç”¨é€—å·åˆ†éš”
--password value            ç”¨äºéäº¤äº’å¼å¯†ç è¾“å…¥çš„å¯†ç æ–‡ä»¶
```
### APIå’Œæ§åˆ¶å°é€‰é¡¹:
```
--rpc                       å¯ç”¨HTTP-RPCæœåŠ¡å™¨
--rpcaddr value             HTTP-RPCæœåŠ¡å™¨æ¥å£åœ°å€(é»˜è®¤å€¼:â€œlocalhostâ€)
--rpcport value             HTTP-RPCæœåŠ¡å™¨ç›‘å¬ç«¯å£(é»˜è®¤å€¼:8545)
--rpcapi value              åŸºäºHTTP-RPCæ¥å£æä¾›çš„API
--ws                        å¯ç”¨WS-RPCæœåŠ¡å™¨
--wsaddr value              WS-RPCæœåŠ¡å™¨ç›‘å¬æ¥å£åœ°å€(é»˜è®¤å€¼:â€œlocalhostâ€)
--wsport value              WS-RPCæœåŠ¡å™¨ç›‘å¬ç«¯å£(é»˜è®¤å€¼:8546)
--wsapi  value              åŸºäºWS-RPCçš„æ¥å£æä¾›çš„API
--wsorigins value           websocketsè¯·æ±‚å…è®¸çš„æº
--ipcdisable                ç¦ç”¨IPC-RPCæœåŠ¡å™¨
--ipcpath                   åŒ…å«åœ¨datadiré‡Œçš„IPC socket/pipeæ–‡ä»¶å(è½¬ä¹‰è¿‡çš„æ˜¾å¼è·¯å¾„)
--rpccorsdomain value       å…è®¸è·¨åŸŸè¯·æ±‚çš„åŸŸååˆ—è¡¨(é€—å·åˆ†éš”)(æµè§ˆå™¨å¼ºåˆ¶)
--jspath loadScript         JavaScriptåŠ è½½è„šæœ¬çš„æ ¹è·¯å¾„(é»˜è®¤å€¼:â€œ.â€)
--exec value                æ‰§è¡ŒJavaScriptè¯­å¥(åªèƒ½ç»“åˆconsole/attachä½¿ç”¨)
--preload value             é¢„åŠ è½½åˆ°æ§åˆ¶å°çš„JavaScriptæ–‡ä»¶åˆ—è¡¨(é€—å·åˆ†éš”)
```
### ç½‘ç»œé€‰é¡¹:
```
--bootnodes value    ç”¨äºP2På‘ç°å¼•å¯¼çš„enode urls(é€—å·åˆ†éš”)(å¯¹äºlight serversç”¨v4+v5ä»£æ›¿)
--bootnodesv4 value  ç”¨äºP2P v4å‘ç°å¼•å¯¼çš„enode urls(é€—å·åˆ†éš”) (light server, å…¨èŠ‚ç‚¹)
--bootnodesv5 value  ç”¨äºP2P v5å‘ç°å¼•å¯¼çš„enode urls(é€—å·åˆ†éš”) (light server, è½»èŠ‚ç‚¹)
--port value         ç½‘å¡ç›‘å¬ç«¯å£(é»˜è®¤å€¼:30303)
--maxpeers value     æœ€å¤§çš„ç½‘ç»œèŠ‚ç‚¹æ•°é‡(å¦‚æœè®¾ç½®ä¸º0ï¼Œç½‘ç»œå°†è¢«ç¦ç”¨)(é»˜è®¤å€¼:25)
--maxpendpeers value    æœ€å¤§å°è¯•è¿æ¥çš„æ•°é‡(å¦‚æœè®¾ç½®ä¸º0ï¼Œåˆ™å°†ä½¿ç”¨é»˜è®¤å€¼)(é»˜è®¤å€¼:0)
--nat value             NATç«¯å£æ˜ å°„æœºåˆ¶ (any|none|upnp|pmp|extip:<IP>) (é»˜è®¤: â€œanyâ€)
--nodiscover            ç¦ç”¨èŠ‚ç‚¹å‘ç°æœºåˆ¶(æ‰‹åŠ¨æ·»åŠ èŠ‚ç‚¹)
--v5disc                å¯ç”¨å®éªŒæ€§çš„RLPx V5(Topicå‘ç°)æœºåˆ¶
--nodekey value         P2PèŠ‚ç‚¹å¯†é’¥æ–‡ä»¶
--nodekeyhex value      åå…­è¿›åˆ¶çš„P2PèŠ‚ç‚¹å¯†é’¥(ç”¨äºæµ‹è¯•)
```

### çŸ¿å·¥é€‰é¡¹:
```
--mine                  æ‰“å¼€æŒ–çŸ¿
--minerthreads value    æŒ–çŸ¿ä½¿ç”¨çš„CPUçº¿ç¨‹æ•°é‡(é»˜è®¤å€¼:8)
--etherbase value       æŒ–çŸ¿å¥–åŠ±åœ°å€(é»˜è®¤=ç¬¬ä¸€ä¸ªåˆ›å»ºçš„å¸æˆ·)(é»˜è®¤å€¼:â€œ0â€)
--targetgaslimit value  ç›®æ ‡gasé™åˆ¶ï¼šè®¾ç½®æœ€ä½gasé™åˆ¶ï¼ˆä½äºè¿™ä¸ªä¸ä¼šè¢«æŒ–ï¼Ÿï¼‰ (é»˜è®¤å€¼:â€œ4712388â€)
--gasprice value        æŒ–çŸ¿æ¥å—äº¤æ˜“çš„æœ€ä½gasä»·æ ¼
--extradata value       çŸ¿å·¥è®¾ç½®çš„é¢å¤–å—æ•°æ®(é»˜è®¤=client version)
```
### GASä»·æ ¼é€‰é¡¹:
```
--gpoblocks value      ç”¨äºæ£€æŸ¥gasä»·æ ¼çš„æœ€è¿‘å—çš„ä¸ªæ•°  (é»˜è®¤: 10)
--gpopercentile value  å»ºè®®gasä»·å‚è€ƒæœ€è¿‘äº¤æ˜“çš„gasä»·çš„ç™¾åˆ†ä½æ•°ï¼Œ(é»˜è®¤: 50)
```
### è™šæ‹Ÿæœºçš„é€‰é¡¹:
```
--vmdebug        è®°å½•VMåŠåˆçº¦è°ƒè¯•ä¿¡æ¯
```
### æ—¥å¿—å’Œè°ƒè¯•é€‰é¡¹:
```
--metrics            å¯ç”¨metricsæ”¶é›†å’ŒæŠ¥å‘Š
--fakepow            ç¦ç”¨proof-of-workéªŒè¯
--verbosity value    æ—¥å¿—è¯¦ç»†åº¦:0=silent, 1=error, 2=warn, 3=info, 4=debug, 5=detail (default: 3)
--vmodule value      æ¯ä¸ªæ¨¡å—è¯¦ç»†åº¦:ä»¥ <pattern>=<level>çš„é€—å·åˆ†éš”åˆ—è¡¨ (æ¯”å¦‚ eth/*=6,p2p=5)
--backtrace value    è¯·æ±‚ç‰¹å®šæ—¥å¿—è®°å½•å †æ ˆè·Ÿè¸ª (æ¯”å¦‚ â€œblock.go:271â€)
--debug                     çªå‡ºæ˜¾ç¤ºè°ƒç”¨ä½ç½®æ—¥å¿—(æ–‡ä»¶ååŠè¡Œå·)
--pprof                     å¯ç”¨pprof HTTPæœåŠ¡å™¨
--pprofaddr value           pprof HTTPæœåŠ¡å™¨ç›‘å¬æ¥å£(é»˜è®¤å€¼:127.0.0.1)
--pprofport value           pprof HTTPæœåŠ¡å™¨ç›‘å¬ç«¯å£(é»˜è®¤å€¼:6060)
--memprofilerate value      æŒ‰æŒ‡å®šé¢‘ç‡æ‰“å¼€memory profiling    (é»˜è®¤:524288)
--blockprofilerate value    æŒ‰æŒ‡å®šé¢‘ç‡æ‰“å¼€block profiling    (é»˜è®¤å€¼:0)
--cpuprofile value          å°†CPU profileå†™å…¥æŒ‡å®šæ–‡ä»¶
--trace value               å°†execution traceå†™å…¥æŒ‡å®šæ–‡ä»¶
```
### WHISPERå®éªŒé€‰é¡¹:
```
--shh                        å¯ç”¨Whisper
--shh.maxmessagesize value   å¯æ¥å—çš„æœ€å¤§çš„æ¶ˆæ¯å¤§å° (é»˜è®¤å€¼: 1048576)
--shh.pow value              å¯æ¥å—çš„æœ€å°çš„POW (é»˜è®¤å€¼: 0.2)
```
### å¼ƒç”¨é€‰é¡¹ï¼š
```
--fast     å¼€å¯å¿«é€ŸåŒæ­¥
--light    å¯ç”¨è½»å®¢æˆ·ç«¯æ¨¡å¼
>>>>>>> new articles
>>>>>>> new articles
```
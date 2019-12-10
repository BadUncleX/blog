---
layout: post
title:  " 以太坊 testrpc "
date:   2018-07-18 05:11:46 +0800
categories: 以太坊 testrpc 100days
typora-copy-images-to: ../assets/images/2018-07
typora-root-url: ../../blog_alexcode
---
以太坊 testrpc


* TOC
{:toc}
geth是真正的以太坊环境, testrpc是在本地使用内存模拟的一个以太坊环境, 这对于开发调试来说更为方便快捷, 因为每次都是一个全新的环境, 便于重复测试.   一般可以将你的合约首先在testrpc中测试通过后, 再部署到geth中. 



## 安装testrpc

```bash
npm install -g ethereumjs-testrpc
```



> 提示已经renamed to ganache-cli





## 运行testrpc

```bash
testrpc
EthereumJS TestRPC v6.0.3 (ganache-core: 2.0.2)

Available Accounts
==================
(0) 0xa49097a2e4d4fe640344ce23548c736b1c502e9d
(1) 0x007354ae5d7dc875db954c758a0d1aea36fb4259
(2) 0xcf3793d531456e58029586ee619bae237df90d0b
(3) 0x6d90ee4b35b9dcab9c1dced1ceba42fc42d6578c
(4) 0x02ccce3dd63e5c549baba0b1dc6111134b6733eb
(5) 0xf24883f1310192a3c49c1649c0238e7b50ccca0a
(6) 0xbffc97eb11bd641efd9ba87a1ee816705ceef4e9
(7) 0x9599a604850b092649de8b3e2180451911146fba
(8) 0x76e7c349c6680b5e7c1d696c0086ee269421fcfd
(9) 0xbc7f2184d6e15aa9b2b1f12626b684f9273ded15

Private Keys
==================
(0) 56d1e3a6d55a9863676f877c9939808e9caae20eaa2529a34f29d23d6719be92
(1) adc849348a9e1530f2cdc4acf3304f616a263ddf27159c90515d7c99c0ac1066
(2) b88faf2b713b0b528283bd930a6884040b77aead6f22b535049020fd0b6c9dfd
(3) 00e5d0cdbd574e9eb7dff17c8228262f58e9b597f8ce5ce10012ebf5a0758b13
(4) 889678e240fc552f7e7157a6c051e1b68fa22e8c8035f23eabdb168cff4d5f2d
(5) 45b4355df608c731612c9cae03556731e84c00730cc41a6e22a206173487d92c
(6) b90fbbbb7d174bff24fb17f026abdd23dc53053e5264dea7d3fdee3c7344a2c9
(7) e8b19309c58bd180ada8b09085a9a292b2556018ed4283b0793fb9b8895f6528
(8) ecd8ea128b4e2583981fe999b1a6570062eb756f73f1929bab6793a0f3c95d3a
(9) a1d11c8d724bc81403e9b3df0d34626e978ccf70f69afb0403206e919a602fd1

HD Wallet
==================
Mnemonic:      spend humor about wife brown style injury echo truck year topic volcano
Base HD Path:  m/44'/60'/0'/0/{account_index}

Listening on localhost:8545
```



默认生成10个帐户, 也可以自己指定, 比如 5个帐户:

```bash
testrpc -a 5
```



## 参数

| 参数                   | 备注                                                         |
| ---------------------- | ------------------------------------------------------------ |
| -a 或者 --accounts     | 启动时生成多少帐户                                           |
| -b 或者 --blocktime    | 多少秒产生一个区块, 用于自动挖矿, 默认为0,不会自动挖矿       |
| -d或者 --deterministic | 产生一个基于之前定义的助记符的确定地址                       |
| -m或者--mnemonic       | 使用一个指定的分层确定钱包助记符来生成初始的地址             |
| -p或者--port           | 监听的端口, 默认为8545                                       |
| -s或者--seed           | 使用任意的数据生成分层确定钱包助记符                         |
| -g或者--gasPrice       | 自定义gas价格                                                |
| -l或者 --gasLimit      | 自定义gas限额                                                |
| -f或者--fork           | 从另一个当前正在运行者的以太坊客户端所给的区块链编号开始分叉, 可以用@符号来指定要分叉的区块编号 |



## testrpc当作第三方库使用

As a Web3 provider:

```js
var ganache = require("ganache-cli");
web3.setProvider(ganache.provider());
```

As a general http server:

```js
var ganache = require("ganache-cli");
var server = ganache.server();
server.listen(port, function(err, blockchain) {...});
```



## testrpc实现的完整方法列表

- `bzz_hive` (stub)
- `bzz_info` (stub)
- `debug_traceTransaction`
- `eth_accounts`
- `eth_blockNumber`
- `eth_call`
- `eth_coinbase`
- `eth_estimateGas`
- `eth_gasPrice`
- `eth_getBalance`
- `eth_getBlockByNumber`
- `eth_getBlockByHash`
- `eth_getBlockTransactionCountByHash`
- `eth_getBlockTransactionCountByNumber`
- `eth_getCode` (only supports block number “latest”)
- `eth_getCompilers`
- `eth_getFilterChanges`
- `eth_getFilterLogs`
- `eth_getLogs`
- `eth_getStorageAt`
- `eth_getTransactionByHash`
- `eth_getTransactionByBlockHashAndIndex`
- `eth_getTransactionByBlockNumberAndIndex`
- `eth_getTransactionCount`
- `eth_getTransactionReceipt`
- `eth_hashrate`
- `eth_mining`
- `eth_newBlockFilter`
- `eth_newFilter` (includes log/event filters)
- `eth_protocolVersion`
- `eth_sendTransaction`
- `eth_sendRawTransaction`
- `eth_sign`
- `eth_syncing`
- `eth_uninstallFilter`
- `net_listening`
- `net_peerCount`
- `net_version`
- `miner_start`
- `miner_stop`
- `personal_listAccounts`
- `personal_lockAccount`
- `personal_newAccount`
- `personal_unlockAccount`
- `personal_sendTransaction`
- `shh_version`
- `rpc_modules`
- `web3_clientVersion`
- `web3_sha3`




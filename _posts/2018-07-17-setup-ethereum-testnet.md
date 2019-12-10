---
layout: post
title:  " 搭建以太坊测试网络 "
date:   2018-07-17 16:42:23 +0800
categories: ethereum 100days
typora-copy-images-to: ../assets/images/2018-07
typora-root-url: ../../blog_alexcode
---
setup ethereum testnet


* TOC
{:toc}
## 安装geth客户端

```bash
brew tap ethereum/ethereum
brew install ethereum
```



## 运行以太坊

1 在以太坊公链上运行一个全节点

```bash
geth --fast --cache=512 --datadir "your pat" console
```



2 在以太坊TestNet上运行一个全节点

```bash
geth --testnet --fast --cache=512 --datadir "your path" console
```





## 搭建一个私有链



### 创世区块配置文件: gensis.json

```json
{
  "config": {
        "chainId": 10, 
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
  "alloc"      : {},
  "coinbase"   : "0x0000000000000000000000000000000000000000",
  "difficulty" : "0x20000",
  "extraData"  : "",
  "gasLimit"   : "0x2fefd8",
  "nonce"      : "0x0000000000000042",
  "mixhash"    : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "timestamp"  : "0x00"
}
```



### 初始化

```bash
geth --datadir "./db" init genesis.json
```





### 启动节点 



```bash
geth --identity "TestNode" --rpc --rpcaddr=0.0.0.0 --rpccorsdomain "*" --rpcport "8545" --datadir "./db" --port "30303" --rpcapi "eth,net,web3,personal,admin,shh,txpool,debug,miner" --nodiscover --maxpeers 30 --networkid 1981 --mine --minerthreads 1 --etherbase "0xeb680f30715f347d4eb5cd03ac5eced297ac5046"  console

```





### javascript 控制台

```bash
geth --datadir './db' attach ipc:./db/geth.ipc
```



```bash
> personal.newAccount("123456")
"0x8c82498549c86043f97712be1081cf750eb25ca9"
```



keystore目录下多了文件: `UTC--2018-07-17T09-16-22.933424637Z--8c82498549c86043f97712be1081cf750eb25ca9`



### 查看账户和余额

```bash
> eth.accounts
["0x8c82498549c86043f97712be1081cf750eb25ca9", "0x58eb51c51864dbfb82db3a9ae0632f18a8d6f716"]
> balance = web3.fromWei(eth.getBalance(eth.accounts[0]), "ether")
0
> balance = web3.fromWei(eth.getBalance(eth.accounts[1]), "ether")
0
>
```



### 挖矿

```js
miner.setEtherbase(eth.accounts[0])

```



```bash
> eth.coinbase
"0x8c82498549c86043f97712be1081cf750eb25ca9"
```





### 停止挖矿

```bash
miner.stop()
```



查看下余额

```javascript
> balance = web3.fromWei(eth.getBalance(eth.accounts[0]), "ether")
8400
```



### 解锁账户

在转账之前需要先解锁账户

```bash
> personal.unlockAccount(eth.accounts[0], "123456")
true
```



### 交易

```bash
> eth.sendTransaction({from: eth.accounts[0], to: eth.accounts[1], value: web3.toWei(1, "ether")})
"0xfe07fae31578808b326525c2723b86270c103aa79c8e30429bd3230c9b60dde2"
```



### 查看交易池等待被打包的交易

```bash
> txpool.status
{
  pending: 1,
  queued: 0
}
```



### 继续挖矿, 打包

```bash
> miner.start(1); admin.sleepBlocks(1); miner.stop()
true

> txpool.status
{
  pending: 0,
  queued: 0
}


> balance = web3.fromWei(eth.getBalance(eth.accounts[1]), "ether")
1
```



### 区块查看

```bash
> eth.getTransaction("0xfe07fae31578808b326525c2723b86270c103aa79c8e30429bd3230c9b60dde2")
{
  blockHash: "0x3e1a243e001c08c8bca3835762ca4cde37050464f2c629753776581a6297911a",
  blockNumber: 2053,
  from: "0x8c82498549c86043f97712be1081cf750eb25ca9",
  gas: 90000,
  gasPrice: 18000000000,
  hash: "0xfe07fae31578808b326525c2723b86270c103aa79c8e30429bd3230c9b60dde2",
  input: "0x",
  nonce: 0,
  r: "0xbfb6094cb50ef14dac863b871d02904dbb4d49470ffe471fdae34af13a0cb03a",
  s: "0x2676f81f888985f258cacf407fb886de362106f59c9390119ccedab946669f96",
  to: "0x58eb51c51864dbfb82db3a9ae0632f18a8d6f716",
  transactionIndex: 0,
  v: "0x37",
  value: 1000000000000000000
}
```



### 区块详细信息查看

```bash
> eth.getTransactionReceipt("0xfe07fae31578808b326525c2723b86270c103aa79c8e30429bd3230c9b60dde2")
{
  blockHash: "0x3e1a243e001c08c8bca3835762ca4cde37050464f2c629753776581a6297911a",
  blockNumber: 2053,
  contractAddress: null,
  cumulativeGasUsed: 21000,
  from: "0x8c82498549c86043f97712be1081cf750eb25ca9",
  gasUsed: 21000,
  logs: [],
  logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  root: "0x0a994712c2997a9bb154fdc1279bd691cc753a846ff315a06ca140b1e0388efb",
  to: "0x58eb51c51864dbfb82db3a9ae0632f18a8d6f716",
  transactionHash: "0xfe07fae31578808b326525c2723b86270c103aa79c8e30429bd3230c9b60dde2",
  transactionIndex: 0
}
```



### 其他一些常用的查询区块的命令

```bash
> eth.blockNumber
2053

```



```bash
> eth.getBlock("latest")
{
  difficulty: 207308,
  extraData: "0xd98301080c846765746888676f312e31302e338664617277696e",
  gasLimit: 4712388,
  gasUsed: 21000,
  hash: "0x3e1a243e001c08c8bca3835762ca4cde37050464f2c629753776581a6297911a",
  logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  miner: "0x8c82498549c86043f97712be1081cf750eb25ca9",
  mixHash: "0xf2e3afded145c594a87e66b9a708d22f51264feec282084371ba96ffe46ae6ce",
  nonce: "0x0d36b9a3b96ee86d",
  number: 2053,
  parentHash: "0x50db0d5e87c1e86fbe356c0083c777647cd93dc67c920f65069e03da11cdaa7c",
  receiptsRoot: "0x4045ac6880c9e1bb65863477ea5ff5ac9192fb09bb0359f439c294ae25839a5f",
  sha3Uncles: "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
  size: 653,
  stateRoot: "0x205e8e3f15d97fb14138e349caaef025b302f38d4ec1cbf8de22cad45e9d7113",
  timestamp: 1531835852,
  totalDifficulty: 328873424,
  transactions: ["0xfe07fae31578808b326525c2723b86270c103aa79c8e30429bd3230c9b60dde2"],
  transactionsRoot: "0xd3bf7343145ab8f1d3aae9de072de6db2ca25c1ccfefd37601819b1092addf08",
  uncles: []
}
```


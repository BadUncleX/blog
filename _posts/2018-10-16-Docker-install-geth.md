---
layout: post
title:  " Docker 安装geth"
date:   2018-10-16 19:27:11 +0800
categories: docker geth
typora-copy-images-to: ../assets/images/2018-10
typora-root-url: ../../blog_alexcode
---
Docker 安装geth


* TOC
{:toc}
## 0 构建geth

geth DockerFile：

```dockerfile
from ubuntu:16.04_2
ENV PATH $PATH:/usr/lib/go-1.10/bin
ENV PATH $PATH:/root/go-ethereum/build/bin
RUN apt-get update && add-apt-repository -y ppa:gophers/archive \
    && apt-get update && apt install  -y  build-essential  golang-1.10-go golang-1.10-doc  \
    && cd  \
    && git clone  https://github.com/ethereum/go-ethereum.git \
    && cd go-ethereum \
    && make geth

RUN mkdir /data
COPY Dockerfile /data


VOLUME /root

EXPOSE 8545 8546 30303 30303/udp
ENTRYPOINT ["geth"]


# clean
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /root/*.tar.gz /root/*.tgz /root/*.zip


```

```bash
docker build -t geth -f Dockerfile .
```

> 这里的/root死活没用， 只能用/data了

上面的ubuntu:16.04_2是在 16.04标准包上面做了部分优化， 包括常用软件包， 和本地代理， 以及将apt的数据源切换到阿里的镜像

ubuntu:16.04_2 DockerFile：

```dockerfile
from ubuntu:16.04
ENV https_proxy "http://alexhost.local:6152"
ENV http_proxy "http://alexhost.local:6152"
ENV all_proxy "socks5://alexhost.local:6153"

RUN mv /etc/apt/sources.list /etc/apt/sources.list.backup

#update aliyun mirror for apt-get
COPY sources.list /etc/apt/

RUN apt-get update  && apt-get install -y \
    software-properties-common python-software-properties \
    && apt-get update  && apt-get install -y \
    net-tools iputils-ping \
    build-essential  openssh-server \
    libdb++-dev   libssl-dev  libreadline-dev  autoconf  curl wget vim git

# clean
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /root/*.tar.gz /root/*.tgz /root/*.zip

```







## 1 安装rinkeby 全节点测试网络

> 将data数据放到一个移动硬盘里， 数据较大， 可以慢慢同步



```bash
docker run -it --rm --name rinkeby -v /Volumes/docker/ethereum:/data -p 8545:8545 -p 30303:30303 geth --rpc --rpcaddr "0.0.0.0" --rinkeby console --datadir=/data/rinkeby --ipcpath "~/gethrinkeby.ipc"
```

> volumey直接映射到/root不好使， 只好单独用一个/data 目录， 并指定参数 --datadir 和 --ipcpath； 



## 2 安装 testnet 全节点测试网络



```bash
docker run -it --rm --name rinkeby -v /Volumes/docker/ethereum:/data -p 8545:8545 -p 30303:30303 geth --rpc --rpcaddr "0.0.0.0" --testnet console --datadir=/data/testnet --ipcpath "~/gethtestnet.ipc"
```



## 3 安装一个私有链

> 私有链的数据不大， 就直接放在本地磁盘了

```bash
# 创建账户
geth account new --datadir "/Users/Alex/docker/docker_geth_alextestnode"

//0376a0321bbbd6fb8ec7f9c90894a9317628984a 

ls /Users/Alex/docker/docker_geth_alextestnode/keystore
UTC--2018-10-16T10-53-20.311944625Z--0376a0321bbbd6fb8ec7f9c90894a9317628984a
```





准备配置文件 /Users/Alex/docker/docker_geth_alextestnode/genesis.json:

```json
{
  "config": {
        "chainId": 201807286666, 
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
  "alloc": {
  "0376a0321bbbd6fb8ec7f9c90894a9317628984a": 
  	{ "balance": "0x8000000000000000000" }
   },
  "coinbase"   : "0x0000000000000000000000000000000000000000",
  "difficulty" : "0x20000",
  "extraData"  : "",
  "gasLimit"   : "0x2100000",
  "nonce"      : "0x0000000000000042",
  "mixhash"    : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "timestamp"  : "0x00"
}
```

```bash
# 初始化
geth --datadir "/Users/Alex/docker/docker_geth_alextestnode" init "/Users/Alex/docker/docker_geth_alextestnode/genesis.json"
```





```bash


# 启动节点
docker run -d --rm --name alextestnode -v /Users/Alex/docker/docker_geth_alextestnode:/data -p 8545:8545 -p 30303:30303 geth --rpc --rpcaddr "0.0.0.0" --identity "AlexTestNode" --rpc --rpcaddr=0.0.0.0 --rpccorsdomain "*" --rpcport "8545" --datadir "/data" --ipcpath "/data/gethalextestnode.ipc" --port "30303" --rpcapi "eth,net,web3,personal,admin,shh,txpool,debug,miner" --nodiscover --maxpeers 30 --networkid 201807286666 --mine --minerthreads 1 --etherbase "0376a0321bbbd6fb8ec7f9c90894a9317628984a"


# JavaScript控制台 :
docker exec -it alextestnode /bin/sh

geth --datadir '/data/alextestnode' attach ipc:"/data/gethalextestnode.ipc"

```



## 查看发起人的余额

```bash
> web3.fromWei(eth.getBalance(addr1), "ether")
37798.931862957161709568
> web3.fromWei(eth.getBalance(addr2), "ether")
0

```



## 交易之前， 解锁发起帐号

```bash
personal.unlockAccount(addr1, "123456")
```



## 发起交易

```bash
> eth.sendTransaction({from:addr1, to:addr2, value: web3.toWei(10.0, "ether")})

"0x18bf24b6a1c1ae0f1e64db13c47af563c118fe823708b6d66533ab6e353c59ce"
```





## 发起交易： 向metamask帐户转账

```bash
eth.sendTransaction({from:addr1, to:addr3, value: web3.toWei(10.0, "ether")})
```

![](/assets/images/2018-10/2018-10-16-110440.png)



## review

用docker管理的好处是可沉淀。 



将docker的配置信息放到github， 任何时候只要重新运行docker命令， 即可重新构建环境。 



在我本机上， 要运行rinkeby， 直接输入： 

```bash
geth.rinkeby.start
```



背后做了一个alias：

```bash
## start geth with rinkeby
geth.rinkeby.start(){
    docker run -it --rm --name rinkeby -v /Volumes/docker/ethereum:/data -p 8545:8545 -p 30303:30303 geth --rpc --rpcaddr "0.0.0.0" --rinkeby console --datadir=/data/rinkeby --ipcpath "~/geth.ipc"
}

alias geth.rinkeby.stop="docker stop rinkeby"
```


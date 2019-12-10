---
layout: post
title:  " Solidity语法与Truffle简介"
date:   2018-07-27 22:05:56 +0800
categories: solidity truffle
typora-copy-images-to: ../assets/images/2018-07
typora-root-url: ../../blog_alexcode
---
Solidity语法与Truffle简介


* TOC
{:toc}


动态数组 ，bytes， string

![](/assets/images/2018-07/2018-07-27-141129.jpg)



![](/assets/images/2018-07/2018-07-27-145321.jpg)



合约调用

![](/assets/images/2018-07/2018-07-27-145359.jpg)



## truffle



install nvm

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```



 

install nodejs

```bash
nvm install --lts
```



npm再更新下

```bash
npm install -g npm
```



![](/assets/images/2018-07/2018-07-27-152400.jpg)



> npm版本更新到了6.2





安装truffle

```bash
npm install -g truffle ganache-cli webpack@4.0.0
```



```bash
mkdir hello_world
cd hello_world
truffle unbox webpack
```



运行ganache

```bash
ganache-cli
```



部署合约 

```bash
truffle migrate
```



会报错， 连不上网络， 修改truffle.js, 将端口修改为8545

```bash
vi truffle.js
```

```json
"scripts": {
    "lint": "eslint ./",
    "build": "webpack",
    "dev": "webpack-dev-server --host 0.0.0.0 --port 8008"
  },
```



vi app/javascripts/app.js

将ip改为目标部署地址， 以及端口改为8544





npm install webpack-dev-server -g



报错， 安装webpack-cli

npm install webpack-cli -D



webpack-dev-server --host 0.0.0.0 --port 8008






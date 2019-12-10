---
layout: post
title:  " docker挂在的本地卷权限问题处理"
date:   2018-08-05 20:11:53 +0800
categories: docker 100days
typora-copy-images-to: ../assets/images/2018-07
typora-root-url: ../../blog_alexcode
---
今天在Mac上跑docker的bitcoin全节点同步， 之前空间不够， 这次将空间直接挂在一个2TB的移动硬盘上。


* TOC
{:toc}




晚上到家的时候不知道谁给我Mac合上了， 然后重新启动就报错：

Operation not permitted 



单独将Volume挂到一个新的image里修复也修复不了。 



搞了2小时， 最后终于找到一个有效方式， 就是docker run的时候指定-u参数



```bash
docker run -v /Volumes/docker/bitcoind-data:/bitcoin --name=bitcoind-node -d -u `id -u $USER` \
        -p 8333:8333 \
        -p 127.0.0.1:8332:8332 \
        -e DISABLEWALLET=1 \
        -e PRINTTOCONSOLE=1 \
        -e RPCUSER=xxxx \
        -e RPCPASSWORD=xxxx \
        -e txindex=1 \
        kylemanna/bitcoind
```



参考链接见这里： https://denibertovic.com/posts/handling-permissions-with-docker-volumes/




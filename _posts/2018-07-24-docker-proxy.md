---
layout: post
title:  " Mac环境下为Docker设置代理 "
date:   2018-07-24 09:43:01 +0800
categories: proxy docker 100days
typora-copy-images-to: ../assets/images/2018-07
typora-root-url: ../../blog_alexcode
---
为Docker设置代理


* TOC
{:toc}
昨天整了一天的Docker， 因为后面好多的环境不想在主环境里折腾， 毕竟好多类似的， 总会是有一些细微区别的。 



比如geth环境搭建， 我自己已经在Mac上装好了， 老师的课程里我又想再走一遍， 最好的方式是从一个干净的环境， 并且尽量环境能和老师的一致， 现在来看Docker是最佳的环境了。 



后面需要Docker放比特币， 以太坊， EOS开发环境。 



## 代理



Mac上用的代理客户端是Surge，  进入docker之后要设置下http_proxy和https_proxy, 可以直接从Surge里复制过来



![](/assets/images/2018-07/2018-07-24-015103.jpg)



然后粘贴到docker的容器里即可。 



这里有两个地方需要注意：

1 复制过来的IP默认是127.0.0.1 这时要将IP改为宿主的IP， 最好在docker的容器内部往外ping一下确认

2 Surge默认的监控地址是来自本地的请求， 即127.0.0.1， 现在我们将IP改为宿主的IP Surge实际并不会响应请求的

所以， 需要改一下Surge的配置文件：

![](/assets/images/2018-07/2018-07-24-015524.jpg)

> 这里安全性有点问题， 希望能找到更好的方式。  不过我想了下， 用下面的防火墙配置可以阻挡外部访问， 理论上问题也不大。 



3 配置下防火墙， 允许Docker过来的请求

![](/assets/images/2018-07/2018-07-24-071713.jpg) 



补充， 在终端里貌似要不要export http_proxy 并不是必需的， 我又试了几次， 不用好像也可以， 诡异了哈。 反正Surge的配置和防火墙那边设置下差不多了， 实在不行就再Export http_proxy





继续update：

export http_proxy还是要的。

这里还是很诡异， 不export的话curl www.google.com都正常， 但是执行这句就卡住了：

```bash
add-apt-repository -y ppa:gophers/archive
```



export http_proxy之后就ok了， 当然， export的IP必需是要宿主IP。 




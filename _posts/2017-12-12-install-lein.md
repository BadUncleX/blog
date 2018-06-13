---
layout: post
title:  "Mac install lein with proxy"
date:   2017-12-12 10:21:38 +0800
categories: clojure lein
typora-copy-images-to: ../assets/images/2017-12-12
typora-root-url: ../../blog_alexcode
---
<h2>Table of contents</h2>
* TOC
{:toc}


以前每次搞maven或者sbt都在折腾镜像或者本地应用， 现在觉得这个还是麻烦， 最好直接走代理， 一了百了。 



## Malformed reply from SOCKS 错误

这个应该是SSL证书问题， 在surge里安装相关证书即可. 

![14CF0078-4D08-4665-A17D-4C4C342E767F](/assets/images/2017-12-12/14CF0078-4D08-4665-A17D-4C4C342E767F.png)



## Received fatal alert: protocol_version

这个和jdk版本有关， 我用的是7，切换到8就可以了. 



作为开发人员， 用国外的相关库是日常需求， 最好多准备几个工具， 除了Surge这样的， 最好再加几个vpn， 总有一个能搞定。 少折腾。
---
layout: post
title: Scala学习之准备篇:使用Repox仓库
date: 2017-02-06  -0800
comments: true
categories: [scala repository repox nexus]
---

接触Scala之后必然要使用sbt,这个sbt有点变态,尤其是速度上无法忍受, 尝试了自建Nexus私服以及国内其他私服后终于找到最佳模式:
1. 搭建Repox私服
2. 本地Repository (下载activator) (700多M的福利包)
3. 使用sbt-coursier插件并发下载

## 搭建Repox
简单说Repox就是专门为Scala使用的增强maven仓库.

### 为何SBT这么慢
以下引用Repox的说法:

 - sbt默认所使用的仓库都在国外。
 - 从某一个版本起，sbt默认使用https协议访问仓库。由于众所周知的原因，连往国外的https连接不稳定，而sbt对这种网络连接的不稳定没有相应的发现和重试机制。
 sbt对依赖的请求分为四个步骤：
 1. resolve : 向所有仓库（内置的如typesafe repo, central maven等，以及build中添加的resolvers）逐个发送HEAD请求询问是否收藏有此文件
 2. download : 向收藏了此文件的仓库发送Get请求下载文件
 3. resolve checksum : 向上游仓库发送HEAD请求询问是否有此文件的sha1 checksum
 4. download checksum : 如果HEAD返回200，则下载 sha1 文件并保证从原文件计算出的sha1与checksum文件的内容一致
 - 如果没有组织内私服，那么只在每个开发者的本地~/.ivy2/cache目录下有依赖缓存，无法在多个开发者之间共享。
 - 如果我们需要源码包（这对使用ide的开发者很正常），sbt连巨大的javadoc包也要一起下载。
 - 各种bug

### 安装Repox

git clone https://github.com/Centaur/repox.git
cd repox/src/main/resources/admin
bower install
cd ../../../..
sbt assembly
java -jar target/scala-2.12/repox-assembly-0.1-SNAPSHOT.jar



### 更多相关配置
参见[这里][2]


## 下载Activator
另一个加速工具是直接下载Activator, 有700多M, 可以直接引用作为本地仓库.
1. 到[官方][3]下载
2. 解压
3. 配置: .sbt/repositories
```yml
activator: file:///users/Alex/Applications_Data/activator-dist-1.3.12/repository/, [organization]/[module]/(scala_[scalaVersion]/)(sbt_[sbtVersion]/)[revision]/[type]s/[artifact](-[classifier]).[ext]
```

## 配置sbt-coursier并发下载插件
/.sbt/0.13/plugins/build.sbt
```scala
    addSbtPlugin("io.get-coursier" % "sbt-coursier" % "1.0.0-M15")
```

好了,开始飞吧.

[2]:	https://github.com/Centaur/repox/wiki/%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97
[3]:	https://www.lightbend.com/activator/download

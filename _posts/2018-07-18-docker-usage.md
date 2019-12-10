---
layout: post
title:  " Docker使用 "
date:   2018-07-18 06:23:34 +0800
categories: docker 100days
typora-copy-images-to: ../assets/images/2018-07
typora-root-url: ../../blog_alexcode
---
mac下docker使用


* TOC
{:toc}


## mac安装Docker

下载docker for mac dmg包, 拖到Applications完事



## 配置加速器镜像

![](/assets/images/2018-07/2018-07-17-225014.png)



## 删除所有Exit状态的镜像

```bash
docker ps -a | grep Exit | cut -d ' ' -f 1 | xargs docker rm
```



## 导入导出容器

```bash
docker export -o test_for_run.tar ce5

docker import test_for_run.tar - test/ubuntu:v1.0
```



## 常用参数

-it 交互式运行



-d 守护太运行



-v 保存本地映像文件



## 创建镜像

方法一: docker commit命令

比如:

```bash
docker run -it ubuntu:14.04 /bin/bash
touch test
exit
```

```bash
docker commit -m "added a new file" -a "Docker newbee" a925cb40b3f0 test:0.1
```



方法二: 本地模板导入

```bash
cat ubuntu-14.04-x86_64-minimal.tar.gz | docker import - ubuntu:14.04
```



## 存出和载入镜像

```bash
docker save -0 ubuntu_14.04.tar ubuntu:14.04

docker load --input ubuntu_14.04.tar
```



## docker tag

```bash
docker tag hub.c.163.com/public/ubuntu:14.04 ubuntu:14.04

```





## 其他常用命令

```bash
docker images

docker create -it ubuntu:latest

docker start ubuntu:latest

docker run ubuntu:latest /bin/echo 'hello world'

docker ps -a

docker stop containerid

docker rm containerid

docker rmi imageid

docker attach containerid

docker exec -it containerid /bin/bash
```





## 搭建本地私有仓库

```bash
docker run -d -p 5000:5000 -v /Users/alex/opt/docker/registry:/tmp/registry registry
```


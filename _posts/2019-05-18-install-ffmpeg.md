---
layout: post
title:  " video/audio convert with ffmpeg"
date:   2019-05-18 18:04:56 +0800
categories: video audio mac
typora-copy-images-to: ../assets/images/2018-10
typora-root-url: ../../blog_alexcode
---
<h2>Table of contents</h2>
* TOC
{:toc}


##  begin

之前Mac上一直用的一个很好用的视频格式转换工具是Bigasoft Flac Converter, 最近mac系统升级之后卡的一B， 看Bigasoft也不再更新软件了， 所以想到直接用命令行工具吧， ffmpeg使用范围还比较广， 就它了。



##  install

```bash
brew install ffmpeg
brew upgrade ffmpeg
```



遇到错误信息：



```
Error: An unexpected error occurred during the `brew link` step
The formula built, but is not symlinked into /usr/local
Permission denied @ dir_s_mkdir - /usr/local/Frameworks
Error: Permission denied @ dir_s_mkdir - /usr/local/Frameworks
```

Google的解决方案：

```bash
sudo chown -R $(whoami) $(brew --prefix)/*
brew upgrade ffmpeg
```



## convert



```bash
ffmpeg -i meetup.mp4 -f mp3 -ab 192000 -vn yizhibo.mp3
```



更详细的ffmpeg使用参考  [Converting Audio into Different Formats / Sample Rates](https://gist.github.com/whizkydee/804d7e290f46c73f55a84db8a8936d74)



## speed up 

如果想要实现变速：

```bash
ffmpeg -i input.mkv -filter:a "atempo=2.0" -vn output.mkv

```



详细参考这里： [Speeding up/slowing down video audio ](https://trac.ffmpeg.org/wiki/How%20to%20speed%20up%20/%20slow%20down%20a%20video)



## trim video/audio

Trim from 00:02:54.583 to the end of the file

```bash
ffmpeg -i input.mp3 -ss 00:02:54.583 -acodec copy output.mp3
```



Trim from 00:02:54.583 for 5 minutes (300 seconds)

```bash
ffmpeg -i input.mp3 -ss 00:02:54.583 -t 300 -acodec copy output.mp3
```


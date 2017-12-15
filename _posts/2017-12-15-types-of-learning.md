---
layout: post
title:  "机器学习基石 03 types of learning"
date:   2017-12-15 09:26:49 +0800
categories: ml foundation
math: true
typora-copy-images-to: ../assets/images/2017-12
typora-root-url: ../../alexcode
---
<h2>Table of contents</h2>
* TOC
{:toc}


## Learning with Different Output Space



### Multiclass Classification: Coin Recognition Problem 从二元扩展到多元分类



![D74C2407-8577-4898-B560-8D9751AD4173](/assets/images/2017-12/D74C2407-8577-4898-B560-8D9751AD4173.png)

> 二元是多元的一个特例， K = 2
>
> 多元并不是结果有多个，而是从多个中仍然是确定一个， 即结果还是一个.
>
> 应用领域： 识别. 



### Regression: Patient recovery prediction problem

![FFD6D986-2055-4D11-89A1-D17797C6F39A](/assets/images/2017-12/FFD6D986-2055-4D11-89A1-D17797C6F39A.png)

> 多少天恢复 -> regression
>
> y在一个范围 -> bounded regression
>
> 小结： y是实数属于回归问题. 



### Structured Learning: Sequence Tagging Problem

![0ECFE6D2-630F-4642-9E43-D15E5AE54212](/assets/images/2017-12/0ECFE6D2-630F-4642-9E43-D15E5AE54212.png)



> 结构化学习是多元分类的延伸, 可以学习出结构. 
>
> 应用： 
>
> 1. 蛋白质折叠
> 2. 语音解析成文本树





## Learning with different data label



### supervised learning

![E111BFC6-4F48-423A-BA3B-E55D1ECE94DB](/assets/images/2017-12/E111BFC6-4F48-423A-BA3B-E55D1ECE94DB.png)



### Unsupervised learning

![E8EF01A9-B659-4C84-B485-2BB6B17E99BD](/assets/images/2017-12/E8EF01A9-B659-4C84-B485-2BB6B17E99BD.png)

> clustering 聚类问题



![13616F4B-C015-4A67-99EC-245443B502B6](/assets/images/2017-12/13616F4B-C015-4A67-99EC-245443B502B6.png)



> 1. 聚类问题
> 2. 密度估计
> 3. 异常值发现





### Semi-supervised learning

![E7AB8613-6EDC-416E-BA05-88576CFEC9A5](/assets/images/2017-12/E7AB8613-6EDC-416E-BA05-88576CFEC9A5.png)



> 半监督学习： 只给少部分的label



### Reinforcement learning

![1609B7A6-FC1D-45E3-8E27-1A296F31860C](/assets/images/2017-12/1609B7A6-FC1D-45E3-8E27-1A296F31860C.png)



> 增强式学习
>
> 不给显式的label， 通过奖励和惩罚间接说明，宠物训练例子. 



### 练习： 人工标注100张图片是否含有树

![898F5ECD-4142-4847-9427-75C96903E3FF](/assets/images/2017-12/898F5ECD-4142-4847-9427-75C96903E3FF.png)



## learning with different protocol



### batch learning

![DD412183-0F07-4C26-99E4-849FE5316FD2](/assets/images/2017-12/DD412183-0F07-4C26-99E4-849FE5316FD2.png)



![E3769466-5CB5-4E30-B9BA-7934F946EF1E](/assets/images/2017-12/E3769466-5CB5-4E30-B9BA-7934F946EF1E.png)

> 一次喂完所有数据



### online learning

![83F9A014-61E2-4A52-AB8D-D4ADA0E07122](/assets/images/2017-12/83F9A014-61E2-4A52-AB8D-D4ADA0E07122.png)

> 持续学习， 上线后还继续根据最新的反馈修正g函数
>
> 常见场景： 垃圾邮件



### active learning

![5FA4E5CB-F15E-42BC-9B92-D32A997103FD](/assets/images/2017-12/5FA4E5CB-F15E-42BC-9B92-D32A997103FD.png)

> 主动学习： 机器反客为主



### mini summary

![1E056D5A-093E-40FA-BBEB-BA368DEB8BFC](/assets/images/2017-12/1E056D5A-093E-40FA-BBEB-BA368DEB8BFC.png)





### 练习， 人工标识部分图片， 机器主动问答案

![0C12489D-BC82-46A7-A0E0-6EBA92BA008A](/assets/images/2017-12/0C12489D-BC82-46A7-A0E0-6EBA92BA008A.png)





## learning with different input space

关于： 特种工程

### concrete features

![32626614-8B6A-4043-852F-9B4E220BF414](/assets/images/2017-12/32626614-8B6A-4043-852F-9B4E220BF414.png)

### raw features

![968102FA-C40D-4601-B118-CE11CF54C563](/assets/images/2017-12/968102FA-C40D-4601-B118-CE11CF54C563.png)

![1AF1A7C5-61F1-4E39-8369-280EF9A805BB](/assets/images/2017-12/1AF1A7C5-61F1-4E39-8369-280EF9A805BB.png)



> raw features: 图像灰度数据
>
> 比具体数值的feature更难. 



### abstract features

![8E5FE3B9-15F8-473A-B645-B4BEFA1A608F](/assets/images/2017-12/8E5FE3B9-15F8-473A-B645-B4BEFA1A608F.png)

> 抽象feature， 没有物理意义， 对机器学习更加困难
>
> 推荐系统. 



### mini summary

![1362F257-3B38-405D-8C72-9DA795DE3881](/assets/images/2017-12/1362F257-3B38-405D-8C72-9DA795DE3881.png)



### 练习： 在线广告系统推荐最相关图片



![6E7D3083-208B-404A-88A7-B09F8C02A11B](/assets/images/2017-12/6E7D3083-208B-404A-88A7-B09F8C02A11B.png)

> concrete: 用户信息
>
> raw: 图像灰度信息
>
> abstract： 可能的相关性



## Summary

![357D8BE1-66B5-47DE-8C85-2338081BA4D1](/assets/images/2017-12/357D8BE1-66B5-47DE-8C85-2338081BA4D1.png)
---
layout: post
title:  "机器学习基石 01 the learning problem"
date:   2017-12-12 18:54:57 +0800
categories: ml foundation
math: true
typora-copy-images-to: ../assets/images/2017-12-11
typora-root-url: ../../alexcode
---
<h2>Table of contents</h2>
* TOC
{:toc}
## course introduction

![9B62A900-6E80-4F78-99C7-825F4260BD2A](/assets/images/2017-12-11/9B62A900-6E80-4F78-99C7-825F4260BD2A.png)



> 前八周是foundation(mathematics basic), 后七周是techniques (machine learning algorithms)



## what is machine learning

### from learning to machine learning

learning: 

observations $$ \rightarrow $$ learning $$\rightarrow$$ skill

machine learning:

data $$\rightarrow$$ ml $$\rightarrow$$ skill



### why machine learning

- 'define' trees is difficult
- ML-based tree recognition system can be easier to build than hand-programmed system

### machine learning route

- 星际导航
- 视觉/语音识别
- 高频交易
- 个性化服务
- 金融欺诈识别

### key essence of machine learning

- 存在潜在模式可以被学习， 有明确的performance measure
- 这个模式不能人工编程直接实现， （无法给出具体的definition）
- 需要data来feed



## application of machine learning



### 衣食住行

![0E7C6AE3-C551-4BC9-B09E-0B8BE765ACC7](/assets/images/2017-12-11/0E7C6AE3-C551-4BC9-B09E-0B8BE765ACC7.png)

### 教育， 在线出题

![7754798C-6B46-41E3-9F87-A9C9994D6C7E](/assets/images/2017-12-11/7754798C-6B46-41E3-9F87-A9C9994D6C7E.png)



### 娱乐： 推荐系统（movie, music）

![E04C0114-01A6-44E2-B093-4C2B40D4CC2A](/assets/images/2017-12-11/E04C0114-01A6-44E2-B093-4C2B40D4CC2A.png)



![53958998-4A7F-40A1-89C0-E1E944F23AB0](/assets/images/2017-12-11/53958998-4A7F-40A1-89C0-E1E944F23AB0.png)



> 两个维度(viewer, movie)的向量匹配



### Fun time

![54D79794-F02A-4DCB-9B6B-054FC5497C7E](/assets/images/2017-12-11/54D79794-F02A-4DCB-9B6B-054FC5497C7E.png)

![53BBDD8E-6108-4AE9-A89C-257CEA0575D9](/assets/images/2017-12-11/53BBDD8E-6108-4AE9-A89C-257CEA0575D9.png)



## component of machine learning

### 银行发放信用卡评估 example

![85D49760-8BE5-49AC-A03E-C95B5FD54F29](/assets/images/2017-12-11/85D49760-8BE5-49AC-A03E-C95B5FD54F29.png)



![6942B4DD-F93C-4921-91BB-048E096FA97F](/assets/images/2017-12-11/6942B4DD-F93C-4921-91BB-048E096FA97F.png)

> f 目标函数， g 假设函数， 这二者有何区别？



![F4C478E6-6339-4FC5-B30C-C6872BD78490](/assets/images/2017-12-11/F4C478E6-6339-4FC5-B30C-C6872BD78490.png)



![665E796A-4D14-4313-9636-4B46A0FF3EC6](/assets/images/2017-12-11/665E796A-4D14-4313-9636-4B46A0FF3EC6.png)



> model 模型包含A和H， 即演算法和假设函数



### practical definition of machine learning



![ABBA9DCF-B0BA-4DF7-B363-AD58314FB06C](/assets/images/2017-12-11/ABBA9DCF-B0BA-4DF7-B363-AD58314FB06C.png)



### 使用符号描述歌曲推荐模型

![6F1E2DC3-60E8-46DA-9F3C-7CE2F6A68A12](/assets/images/2017-12-11/6F1E2DC3-60E8-46DA-9F3C-7CE2F6A68A12.png)

> $$S_4$$ training data; $$S_3$$公式()，<font color="red">??</font> $$S_2$$预测时输入的data， $$S_1$$预测结果



## machine learning and data mining

### Machine Learning and Data Mining

![0C61EA19-389F-464F-807C-A868AEC3BD9E](/assets/images/2017-12-11/0C61EA19-389F-464F-807C-A868AEC3BD9E.png)

### Machine Learning and Artificial Intelligence

![1AD6EB9C-8413-4A9B-B68A-F2B0625448D5](/assets/images/2017-12-11/1AD6EB9C-8413-4A9B-B68A-F2B0625448D5.png)

### Machine Learning and Statistics

![4EB44A77-3FB0-4B87-A2BE-D7769EDAB3BA](/assets/images/2017-12-11/4EB44A77-3FB0-4B87-A2BE-D7769EDAB3BA.png)



## summary



![B9C36E83-0BAF-4943-81F0-C62C2FEA785D](/assets/images/2017-12-11/B9C36E83-0BAF-4943-81F0-C62C2FEA785D.png)
---
layout: post
title:  "机器学习基石 02  learning to answer yes no"
date:   2017-12-14 10:19:14 +0800
categories: ml foundation
math: true
typora-copy-images-to: ../assets/images/2017-12
typora-root-url: ../../alexcode
---
<h2>Table of contents</h2>
* TOC
{:toc}


![C1F11ABA-336E-4703-AC29-21B4CCB8BB9B](/assets/images/2017-12-12/C1F11ABA-336E-4703-AC29-21B4CCB8BB9B.png)

> review: A takes D and H to get g. D: data， 众多的假设函数H中选出最终的g



问题提出： 大H到底什么样的?

## Perceptron Hypothesis Set

Perceptron: 感知器

![6AEB5B91-A4D2-4ABB-9222-1EA4F2CF80F6](/assets/images/2017-12-12/6AEB5B91-A4D2-4ABB-9222-1EA4F2CF80F6.png)

> h(x)叫 Perceptron 感知器， 来源于神经网络的叫法
>
> 感知器， 终于怯魅了



### 感知器， 向量内积形式

![DE1500F5-D68B-4251-B982-4C1CA84F556A](/assets/images/2017-12-12/DE1500F5-D68B-4251-B982-4C1CA84F556A.png)

> threshold 作为w0, x0为1



提问：h是什么？



### Perceptrons in R^2

![3463BC66-E2D3-4961-A16A-38183AEB12F2](/assets/images/2017-12-12/3463BC66-E2D3-4961-A16A-38183AEB12F2.png)

> 式中， x代表每一个点， y代表+1/-1（圈或者X）, h(x)代表一条直线
>
> perceptron： 线性分类器



### 练习: 在垃圾邮件中， 下面那些单词对于一个好的线性分类器来说有较大的权重

![28319884-812D-411F-9677-354E05799101](/assets/images/2017-12-12/28319884-812D-411F-9677-354E05799101.png)





## Perceptron learning algorithm(PLA)

问题提出： 如何选择一条最好的线 g（从所有的可能分类器H中）



![19AAF503-021C-4FF4-85F4-DD450A6A8BB1](/assets/images/2017-12-12/19AAF503-021C-4FF4-85F4-DD450A6A8BB1.png)



> 但是g有无数个， 我们先从一个随机的初始$$g_0$$开始， 然后不断的**修正** (修正 = 减少误差)



### 如何修正错误

![AD7405BE-6142-4CF1-ACB5-33541DFC3E68](/assets/images/2017-12-12/AD7405BE-6142-4CF1-ACB5-33541DFC3E68.png)

> 1. 向量上的理解：
> 2. y是正的， 但是得到了负的， 说明w与x的角度太大， w+x（往中间合）
> 3. y是负的， 但是得到了正的， 说明w与x的角度太小， w-x（让角度变大）
> 4. 结论： 如果y是正的， 我们就让w靠近x，反之让w远离x    $$W_{t+1} \leftarrow W_t + Y_{n(t)}X_{n(t) }$$
> 5. 最后找到的w叫$$W_{PLA}$$ （PLA perceptron learning algorithm 感知器学习算法）
> 6. 知错能改， 善莫大焉



### 练习

![29628F33-4474-43F6-B32C-75129EEF6BAD](/assets/images/2017-12/29628F33-4474-43F6-B32C-75129EEF6BAD.png)

> 式子两边同时乘以$$y_nx_n$$, -> $$W_{t+1}y_nx_n = (W_t+y_nx_n)y_nx_n$$



## Guarantee of PlA

### Linear separability 线性可分

![6E6DCFC7-7F99-4430-A09B-11605550ACC8](/assets/images/2017-12/6E6DCFC7-7F99-4430-A09B-11605550ACC8.png)

> 1. 线性可分
> 2. 有异常点
> 3. 不是线性的



### PLA Fact: $W_t$ Gets More Aligned with $W_f$

![0CAE3769-C143-4D0F-AA1B-4607AD2E76B1](/assets/images/2017-12/0CAE3769-C143-4D0F-AA1B-4607AD2E76B1.png)



> 1. 数学上证明$$W_t$$ 越来越接近$$W_f$$
> 2. 每一次更新$$W_{t+1} > W_t$$  （表示夹角越来越小）
> 3. 知识点： 向量内积为$$a^Tb = |a| \cdot|b|\cdot cos\alpha$$ ($$\alpha$$ 是a和b的夹角)
>
> 





> 补充： [百度百科 内积](https://baike.baidu.com/item/%E7%82%B9%E7%A7%AF/9648528?fromtitle=%E5%86%85%E7%A7%AF&fromid=422863)
>
> 向量的点积与它们夹角的余弦成正比，如果点积越大，说明夹角越小
>
> ![17259173-FF8E-4904-A8C7-59E7A512528B](/assets/images/2017-12/17259173-FF8E-4904-A8C7-59E7A512528B.png)



### PLA fact: $W_t$ does not grow too fast



![C5B98155-4579-49C2-8CA5-71AE754E7890](/assets/images/2017-12/C5B98155-4579-49C2-8CA5-71AE754E7890.png)

> 数学推导， $$W_t$$不会增长太快？ 
>
> <font color="red">not sure</font>
>
> @problem



> 前面一个说$$W_t$$ 和$$W_f$$越来越靠近， 这一个是说$$W_t$$慢慢的长，将两项结合起来， 看到两个正规化向量内积， 又向量的内积最大为1， 所以会最终停下来. 

> @todo <font color="red">这里的constant是多少?</font>



### 练习： PLA需要更新多少次才会停下来

![2A434929-A19C-457A-A5EE-D5F16406D5CC](/assets/images/2017-12/2A434929-A19C-457A-A5EE-D5F16406D5CC.png)

> <font color="red">没听懂</font>
>
> @problem



## Non-Separable Data

### review 回顾

![7326E760-C5F9-4C39-8682-4AB828B73D5F](/assets/images/2017-12/7326E760-C5F9-4C39-8682-4AB828B73D5F.png)

> 1. 建立在假设是线性可分的基础上， 如果不是线性可分呢
> 2. R是资料的长度， 可以算出来
> 3. $$\rho$$ 是什么？ 是和$$W_f$$有关， 但是我们不知道$$W_f$$





### learning with Noisy data

![4BA0E368-5569-4070-87C9-E64AC05B5045](/assets/images/2017-12/4BA0E368-5569-4070-87C9-E64AC05B5045.png)

> 言下之意， 模型还是线性可分的， 不是我模型不对， 是你的数据不正常， 驱除异常我就是线性可分了



![834B6D17-1EEA-4582-B201-02D05AAFBFF6](/assets/images/2017-12/834B6D17-1EEA-4582-B201-02D05AAFBFF6.png)

> 1. 不求完美， 但求更好
> 2. 但数学上， 这是一个NP-hard问题



### 方案： Pocket Algorithm

![5B3753DE-19BA-4617-922C-DF396B7A4553](/assets/images/2017-12/5B3753DE-19BA-4617-922C-DF396B7A4553.png)

> 贪心算法， 放一个算法在Pocket， 骑驴找马， 有更好的就替换， 跑了一定次数就结束。 
>
> 好”豁达“的算法



### 练习： 如果用Pocket算法， 最终也证明确实是线性可分的，那么浪费了什么？



![CF2C4DFB-E2EC-458E-B14F-7EF5FE13AB95](/assets/images/2017-12/CF2C4DFB-E2EC-458E-B14F-7EF5FE13AB95.png)

> Pocket比PLA慢。 
>
> 原因有：
>
> 1. 要保存
> 2. 要全量比较后才知道哪个更好



## summary

![C71F0303-55D0-4299-B85A-2A621178A8EA](/assets/images/2017-12/C71F0303-55D0-4299-B85A-2A621178A8EA.png)


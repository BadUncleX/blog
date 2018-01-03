---
layout: post
title:  "01 Giving Computers the Ability to Learn from Data"
date:   2018-01-01 11:53:28 +0800
categories: ml python
math: true
typora-copy-images-to: ../assets/images/2018-01
typora-root-url: ../../alexcode
---
<h2>Table of contents</h2>
* TOC
{:toc}
## outline:

1. The general concepts of machine learning
2. The three types of learning and basic terminology

3. The building blocks for successfully designing machine learning systems

4. Installing and setting up Python for data analysis and machine learning



## 监督学习 supervised learning

Making predictions about the future with supervised learning



![B2F3AC1E-E8D6-49F5-9BBD-412DB7DD362B](/assets/images/2018-01/B2F3AC1E-E8D6-49F5-9BBD-412DB7DD362B.png)



监督学习分为两类：

- classification
- regression



classification:

- 二分类， 比如垃圾邮件识别
- 多分类， 比如手写数字识别



![BD6BD3CD-431A-4AE2-89B6-1A4D1E1DC5B9](/assets/images/2018-01/BD6BD3CD-431A-4AE2-89B6-1A4D1E1DC5B9.png)

> 根据给出的x1和x2数据， 学习出一条分界线， 将数据分为两个category



![69CC7D71-87BE-4EBD-86A8-C65AA2B1531D](/assets/images/2018-01/69CC7D71-87BE-4EBD-86A8-C65AA2B1531D.png)

> 根据变量x以及x对应的变量y， 得到一条直线， 该直线与各个点之间的距离最小。  使用学习到的斜率和截距可以预测新的数据。 





Training and selecting a predictive model

how do we know which model performs well on the final test dataset and real-world data if we don't use this test set for the model selection, but keep it for the final model evaluation?

> 使用不同的cross-validation 方法进一步将training dataset 切分成训练集和验证集



## install packages for python



skip.




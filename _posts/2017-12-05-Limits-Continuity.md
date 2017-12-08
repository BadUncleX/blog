---
layout: post
title:  "MIT 18.01单变量微积分02 极限和连续"
date:   2017-12-05 11:34:24 +0800
categories: calculous mit
---
<h2>Table of contents</h2>
* TOC
{:toc}

## one-sided limits
[see here](https://ocw.mit.edu/courses/mathematics/18-01-single-variable-calculus-fall-2006/readings/c_cntnt_dscntnt.pdf)
![image](https://user-images.githubusercontent.com/150418/33607069-a4cc39e2-d9fa-11e7-8e44-6d1c455cadd0.png)

### example
![image](https://user-images.githubusercontent.com/150418/33607082-b9c1ccfe-d9fa-11e7-9589-9845d6e72ac9.png)

![image](https://user-images.githubusercontent.com/150418/33607100-cabbb81c-d9fa-11e7-953c-3be51517bab6.png)

> 左右两侧极限相等， 连续的

![image](https://user-images.githubusercontent.com/150418/33607117-e02b559a-d9fa-11e7-87a2-028d22d4ffc6.png)

> 左右两侧极限不相等， 所以在0点处不可微， 类似的例子还有 f(x) = |x|:

![image](https://user-images.githubusercontent.com/150418/33607846-64af5b48-d9fd-11e7-95e8-115c840b9b6d.png)

![image](https://user-images.githubusercontent.com/150418/33607126-ea79ec1e-d9fa-11e7-9125-079cc5974a13.png)

> x 为0 的时候1/x为无穷大是不严谨的.

![image](https://user-images.githubusercontent.com/150418/33607130-f34a29a8-d9fa-11e7-844b-eb7b4dbf3129.png)


## “rate of change" of derivative
![image](https://user-images.githubusercontent.com/150418/33606541-c57acdae-d9f8-11e7-8b6c-f16a4e73ed87.png)



![image](https://user-images.githubusercontent.com/150418/33606568-d735f258-d9f8-11e7-8adf-998a8f311610.png)


## limits and continuity
### removable discontinuity
![image](https://user-images.githubusercontent.com/150418/33606705-507b7b60-d9f9-11e7-8e06-edcf932b60c2.png)

### Jump discontinuity
![image](https://user-images.githubusercontent.com/150418/33606724-6f825164-d9f9-11e7-9807-c201260caaaf.png)


### Infinit discontinuity
![image](https://user-images.githubusercontent.com/150418/33606733-7d071478-d9f9-11e7-8f44-141b92582364.png)

> 虽然f(x)在负无穷到0， 以及0到正无穷的范围都是连续的， 但是f(x)自身仍然不是连续的

and tanx is also infinit discontinuity
![image](https://user-images.githubusercontent.com/150418/33607525-71709dca-d9fc-11e7-9a67-2579ac36448c.png)


### Other(ugly) discontinuity
![image](https://user-images.githubusercontent.com/150418/33606737-85e25ada-d9f9-11e7-8850-c0b22f282a11.png)

> 结论： f(x)连续的条件： x无限接近a, f(x)的极限等于f(a)

> 结论： 连续和可微的关系, f(x)在a点可微， 则f(x)在a点连续， 反之， f(x)在a点连续， 则f(x)在a点可微， 如f(x) = |x|: (在0点不可微， 所以不连续 -- 左右极限不等)

![image](https://user-images.githubusercontent.com/150418/33607846-64af5b48-d9fd-11e7-95e8-115c840b9b6d.png)



## 三角函数极限


![image](https://user-images.githubusercontent.com/150418/33589604-8c0bc864-d9b4-11e7-899a-457dfeae99d9.png)

> 参考 [这里](https://www.youtube.com/watch?v=KyisKDC98To)

## 中值定理
![image](https://user-images.githubusercontent.com/150418/33608707-50d84636-da00-11e7-8b2f-dcd99ad7fd57.png)

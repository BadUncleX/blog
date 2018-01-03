---
layout: post
title:  "Training Simple Machine Learning Algorithms for Classification"
date:   2018-01-02 08:37:12 +0800
categories: ml python
math: true
typora-copy-images-to: ../assets/images/2018-01
typora-root-url: ../../alexcode
---
<h2>Table of contents</h2>
* TOC
{:toc}


## outline

1. Building an intuition for machine learning algorithms
2. Using pandas, NumPy, and Matplotlib to read in, process, and visualize data

3. Implementing linear classification algorithms in Python



## Artificial neurons – a brief glimpse into the early history of machine learning



![B173F917-98BC-4E29-9584-A50FA257B9CF](/assets/images/2018-01/B173F917-98BC-4E29-9584-A50FA257B9CF.png)

McCullock and Pitts described such a nerve cell as a simple logic gate with **binary** outputs; multiple signals arrive at the **dendrites**, are then integrated into the cell body, and, if the accumulated signal exceeds a certain **threshold**, an **output signal** is generated that will be passed on by the **axon**.



> 随后， 诞生了perceptron算法





## perceptron learning rule



### 感知机学习过程

1. Initialize the weights to 0 or small random numbers.
2. For each training sample $$x^{(i)}$$
   1. Compute the output value y predict
   2. Update the weights



![332C5EAE-F0B4-496B-A966-673B0035BEB3](/assets/images/2018-01/332C5EAE-F0B4-496B-A966-673B0035BEB3.png)

![EC1E04E7-F282-45BB-98B2-6919AC4DF252](/assets/images/2018-01/EC1E04E7-F282-45BB-98B2-6919AC4DF252.png)

![5D6E3B8A-DCC1-458B-8513-A015A553D7E9](/assets/images/2018-01/5D6E3B8A-DCC1-458B-8513-A015A553D7E9.png)

![B9A64F13-87C0-41C9-985C-8DBBBD5CBA8D](/assets/images/2018-01/B9A64F13-87C0-41C9-985C-8DBBBD5CBA8D.png)

![A3F481BE-EFA8-4800-BDA2-08810B81F758](/assets/images/2018-01/A3F481BE-EFA8-4800-BDA2-08810B81F758.png)

> 判断正确的情况下$$\Delta w$$为0
>
> 权重w和x的大小成正比





### 感知机融合的前提

不同类别是线性可分的； 并且学习率足够小





### summary

![CDD7AFAF-AEAD-4B8F-8FC9-4B6BEB52CA37](/assets/images/2018-01/CDD7AFAF-AEAD-4B8F-8FC9-4B6BEB52CA37.png)

> The preceding diagram illustrates how the perceptron receives the inputs of a sample x and combines them with the weights w to compute the **net input**. The net input is then passed on to the **threshold function**, which generates a binary output -1 or +1the predicted class label of the sample. During the learning phase, this output is used to calculate the **error** of the prediction and update the **weights**.



## Implementing a perceptron learning algorithm in Python



### Perceptron class in python

```python
import numpy as np


class Perceptron(object):
    """Perceptron classifier.

    Parameters
    ------------
    eta : float
      Learning rate (between 0.0 and 1.0)
    n_iter : int
      Passes over the training dataset.
    random_state : int
      Random number generator seed for random weight
      initialization.

    Attributes
    -----------
    w_ : 1d-array
      Weights after fitting.
    errors_ : list
      Number of misclassifications (updates) in each epoch.

    """
    def __init__(self, eta=0.01, n_iter=50, random_state=1):
        self.eta = eta
        self.n_iter = n_iter
        self.random_state = random_state

    def fit(self, X, y):
        """Fit training data.

        Parameters
        ----------
        X : {array-like}, shape = [n_samples, n_features]
          Training vectors, where n_samples is the number of samples and
          n_features is the number of features.
        y : array-like, shape = [n_samples]
          Target values.

        Returns
        -------
        self : object

        """
        rgen = np.random.RandomState(self.random_state)
        self.w_ = rgen.normal(loc=0.0, scale=0.01, size=1 + X.shape[1])
        self.errors_ = []

        for _ in range(self.n_iter):
            errors = 0
            # zip usage
            for xi, target in zip(X, y):
                # update: delta w
                update = self.eta * (target - self.predict(xi))
                self.w_[1:] += update * xi

                # self.w_[0] : bias unit
                self.w_[0] += update

                # int(bool condition) usage
                errors += int(update != 0.0)
            self.errors_.append(errors)
        return self

    def net_input(self, X):
        """Calculate net input"""
        return np.dot(X, self.w_[1:]) + self.w_[0]

    def predict(self, X):
        """Return class label after unit step"""
        return np.where(self.net_input(X) >= 0.0, 1, -1)
```



### Training a perceptron model on the Iris dataset


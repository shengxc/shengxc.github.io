---
layout: post
title: "局部加权回归和logistic回归"
description: "斯坦福大学机器学习公开课笔记"
category:
tags: ["机器学习"]
---

#欠拟合（underfitting）和过拟合（overfitting）#
假设下图中的离散点为某地区房子大小（横轴）与房子价格（纵轴）之间的关系。
![拟合][1]
左图中的直线是对数据进行线性回归的结果，中间的图是把直线换成二次曲线所得的结果，而右边的是用5次曲线来拟合得到的完美通过每个坐标点的曲线（由于一共只有6个数据点，因此，5次曲线能够完美通过所有数据点）。左图拟合结果（欠拟合）中的直线无法很好的拟合给定的数据，数据中暗含的某些二次特征，而右图结果（过拟合）仅仅反映了数据本身的特性，而不是隐藏在数据之下的一般变化规律。中间图的拟合结果应该算是这三个中最好的一个。

#参数学习和非参数学习#

##参数学习（parametric learning algorithm）##
参数学习是一类有固定数目的参数可以用来进行数据拟合的算法，线性回归就是一种参数学习方法，其中的$\Theta$就是用来进行拟合的参数。

##非参数学习（non-parametric learning algorithm）##
参数的数目随着训练集合大小的增长而增长的一类机器学习算法。

#局部加权回归（Locally weighted regression）#
局部加权算法的主要思想是通过分段拟合给定的数据点的方法进行对训练数据的拟合。对训练集中每一个拟合目标拟合出$\Theta$，使得$\sum_{i=1}^mw^{(i)}(y^{(i)}-\Theta^Tx^{(i)})^2$最小，其中$w^{(i)}=e^{(-\frac{(x^{(i)}-x)^2}{2})}$，在这种情况下，如果$x^{(i)}-x$很小时，那么$w^{(i)}$将会接近于$1$，否则$w^{(i)}$接近于$0$。这样，距离$x$点比较近的那些点对拟合结果会有较大的影响，而距离$x$较远的点，其影响被相对的忽略了。上述公式中的$w^{(i)}$只是一种常用的系数，可以由其他方法取代。一个一般化的方法是用$w^{(i)}=e^{(-\frac{(x^{(i)}-x)^2}{2\tau})}$来替换原来的$w$，这里的$\tau$是波长参数，其函数图像大致如下。![w函数图像][2] 通过调节$\tau$可以来控制权值$w$随距离变化的速率，如果$\tau$比较小，那么$w$的函数图像将会变得很窄，随着$\tau$的增大，函数图像将会变得越来越平坦。

#logistic回归#

##线性回归解决分类问题##
如果有如下图绿色的点所示数据（x轴代表特征，y轴代表离散的两个类别，红色的直线是对这些数据点进行线性回归所拟合出来的直线）。![直线拟合][3]，在这种情况下，对于一个新的数据点，只要其线性回归的函数预测结果以0.5为分界就可以很好的区分两类数据。但是，如果数据点的分布如下图所示![错误的拟合][4]，那么0.5就无法作为临界值很好的区分这两类数据了。因此，通常情况下用线性回归来分类是个糟糕的选择。

##logistic回归解决分类问题##
假设分类问题的类别$y\in \\{0,1\\}$，logistic回归的拟合函数为$h\_\Theta(X)=g(\Theta^TX)$,
其取值范围为$$h_{\Theta(X)} \in [0,1]$$。
其中，$$g(z)=\frac{1}{1+e^{-z}}$$(函数图像如下)![logstic回归函数图像][5]，因此，$h_\Theta(X)=g(\Theta^TX)=\frac{1}{1+e^{-\Theta^TX}}$

**对logistic回归求解**
在logstic函数中，数据被分成两类的概率如下

$\begin{align}&P(y=1\|X;\Theta)=h\_\Theta(X)\\\\&P(y=0\|X;\Theta)=1-h\_\Theta(X)\end{align}$

上述函数可以写成一个公式$P(y\|X;\Theta)=h\_\Theta(X)^y(1-h\_\Theta(X))^{1-y}$，参数$\Theta$的似然性$L(\Theta)$就是在该参数下数据出现的概率，计算公式如下
$$L(\Theta)=\prod_{i=1}^mP(y^{(i)}|X^{(i)};\Theta)=\prod_{i=1}^{m}h_\Theta(X^{(i)})^{y^{(i)}}(1-h_\Theta(X^{(i)}))^{1-y^{(i)}}$$

对上述公式取对数

$$l(\Theta)=log(L(\Theta))=\sum_{i=1}^m(y^{(i)}log(h_{\Theta}(X^{(i)}))+(1-y^{(i)})log(1-h_\Theta(X^{(i)})))$$

根据最大似然估计，现在的目标是求得$\Theta$使得$l(\Theta)$最大。
这里可以使用类似于梯度下降的梯度上升算法来求$\Theta$，与梯度下降方法不同的是，这里$\Theta$的迭代公式是$\Theta:=\Theta+\alpha\nabla_\Theta{l(\Theta)}$(这个公式与梯度下降的不同之处是中间的+号)，为了导出梯度的上升方向，需要把$l(\Theta)$对每一个$\Theta$分量求偏导。

$$\frac{\partial}{\partial{\Theta_j}}l(\Theta)=\sum_{i=1}^m(y^{(i)}-h_\Theta(X^{(i)}))X_j^{(i)}$$(这个公式我没有自己推导...)

因此，梯度上升迭代过程如下:

$$\Theta_j:=\Theta_j+\alpha\sum_{i=1}^m(y^{(i)}-h_\Theta(X^{(i)}))X_j^{(i)}$$


  [1]: /resource/2014-06-05-machinelearning/curvefit.png
  [2]: /resource/2014-06-05-machinelearning/exp.png
  [3]: /resource/2014-06-05-machinelearning/linefit.png
  [4]: /resource/2014-06-05-machinelearning/wrongfit.png
  [5]: /resource/2014-06-05-machinelearning/logsticfunction.png

{% include JB/setup %}

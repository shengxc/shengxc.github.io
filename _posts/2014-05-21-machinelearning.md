---
layout: post
title: "监督学习:线性回归"
description: "斯坦福大学机器学习公开课笔记"
category:
tags: ["机器学习"]
---

#线性回归（Linear Regression）#

##符号说明##
* $X^{(i)}$:features
* $y^{(i)}$:target
* $(X^{(i)},y^{(i)})$:$i^{th}$training example
* $m$:the number of training examples
* $n$:the size of feature
* $\Theta$：parameter vector

##回归模型##
> 线性回归模型为$$h_{\Theta}(x)=\Theta_0+\sum_{i=1}^{n}\Theta_ix_i$$
> 令$x_0=1$,则特征向量$X=[x_0,x_1,...,x_n]^T$,模型可表示为
> $$h_{\Theta}(x)=\Theta_0+\sum_{i=1}^{n}\Theta_ix_i=\Theta^TX$$
> 机器学习算法的目的是学出上述$\Theta$向量，这样，便可对任意输入$X$向量得到上述$h_{\Theta}(x)$作为预测输出。

##目标函数##
> cost function为$$J(\Theta)=\frac{1}{2}\sum_{i=1}^m(h_{\Theta}(X^{(i)})-y^{(i)})^2$$由此，机器学习算法的目标可描述如下 $$\min_{\Theta}J(\Theta)$$

#解法#

##梯度下降##

###批量梯度下降(Batch gradient descent)###

**基本思想**

> 从某一特定$\Theta$开始，不断改变$\Theta$的取值来降低$J(\Theta)$直到收敛。此算法会收敛到local minimum，不同的初始$\Theta$也有可能收敛到不同的local minimum。（如果是线性回归，$J(\theta)$只有一个最小值）

**算法流程**

> 1. 任意给定一个初始$\Theta$值
> 2. 对于$\Theta$中所有分量，更新$\Theta_i := \Theta_i-\alpha\frac{\partial}{\partial\Theta_i}J(\Theta)$(此步即梯度下降，$\alpha$是步长)
> 3. 当cost function的变化小于某个给定阈值的时候算法停止

**公式推导**

> $$\frac{\partial}{\partial\Theta_i}J(\Theta)=\frac{\partial}{\partial\Theta_i}\frac{1}{2}\sum_{i=1}^m(h_{\Theta}(X^{(i)})-y^{(i)})^2=\sum_{j=1}^m(h_{\Theta}(X^{(j)})-y^{(j)})X^{(j)}_i$$
> 将此公式带入算法流程中的第二步既得完整的算法。

###随机梯度下降(Stochastic gradient descent)###

> 在批量梯度下降算法中，每次迭代都要遍历所有训练样本，当训练样本很多(m很大)时，算法的运行效率会变得很低。随机梯度下降算法会有比批量梯度下降算法快的多的下降速度。

**算法流程**

> 1. 任意给定一个初始$\Theta$值
> 2. 更新$\Theta_i$
> 3. 当cost function的变化小于某个给定阈值的时候算法停止

**说明**

> 随机梯度下降算法跟批量梯度下降算法之间的不同之处在于第2步更新$\Theta$的方法。再随机梯度下降算法中，更新$\Theta$的方法是遍历每一个训练样本$(X^{(j)},y^{(j)})$按照以下公式更新$\Theta$所有的分量。
> $$\Theta_i:=\Theta_i-\alpha(h_{\Theta}(X^{(j)})-y^{(j)})X^{(j)}_i\ (for\ i = 0\ to\ n)$$
> 随机梯度下降是通过每个样本来迭代更新一次，如果样本量很大，那么可能只用其中一部分样本，就已经将$\Theta$迭代到最优解了，对比上面的批量梯度下降，迭代一次需要用到所有训练样本，随机梯度下降算法快的多。但是，该算法并不是每次迭代都向着整体最优化方向下降。

###矩阵方法###
**符号定义**

- $\bigtriangledown\_{\Theta}J=\begin{bmatrix}\frac{\partial{J}}{\partial\Theta_0} \\\\ ... \\\\ \frac{\partial{J}}{\partial\Theta_n}\end{bmatrix} \in R^{n+1}$
    由上述符号，随机梯度下降算法迭代公式可表示为$\Theta=\Theta-\alpha\bigtriangledown_{\Theta}J\ \ (\Theta\in R^{n+1})$
- $f:R^{m\times n}\to R$
- $A \in R^{m\times n}$
- $\bigtriangledown\_{A}f(A)=\begin{bmatrix}\frac{\partial{f}}{\partial A\_{11}}&...& \frac{\partial{f}}{\partial A\_{1n}}\\\\ ...&...&... \\\\ \frac{\partial{f}}{\partial A\_{m1}}&...&\frac{\partial{f}}{\partial A\_{mn}}\end{bmatrix}$
- $X \in R^{n\times n}$
- $trX=\sum\_{i=1}^{n}X_{ii}$ ($trX$表示矩阵$X$的迹)

**不加证明的一些定理**

- $trAB=trBA$
- $trABC=trCAB=trBCA$
- $\bigtriangledown_{A}trAB=B^T$
- $trA=trA^T$
- $tra=a\ (a\in R)$
- $\bigtriangledown_{A}trABA^TC=CAB+C^TAB^T$

**梯度下降的推导**

定义矩阵$$X=\begin{bmatrix}(X^{(1)})^T\\\\...\\\\(X^{(m)})^T\end{bmatrix}$$
根据训练样本构造向量
$$X\Theta=\begin{bmatrix}(X^{(1)})^T\\\\...\\\\(X^{(m)})^T\end{bmatrix}\Theta=\begin{bmatrix}(X^{(1)})^T\Theta\\\\...\\\\(X^{(m)})^T\Theta\end{bmatrix}=\begin{bmatrix}h_\Theta(X^{(1)})\\\\...\\\\h_\Theta(X^{(m)})\end{bmatrix}$$
$$Y=\begin{bmatrix}y^{(1)}\\\\...\\\\y^{(m)}\end{bmatrix}$$
将上述两个向量相减，得

$$X\Theta-Y=\begin{bmatrix}h_\Theta(X^{(1)})-y^{(1)}\\\\...\\\\h_\Theta(X^{(m)})-y^{(m)}\end{bmatrix}$$

$$(X\Theta-Y)^T(X\Theta-Y)=\sum_{i=1}^m(h_\Theta(X^{(i)})-y^{(i)})^2=2J(\Theta)$$

$$\Rightarrow J(\Theta)=\frac{1}{2}(X\Theta-Y)^T(X\Theta-Y)$$

要使$J(\Theta)$取得最小值，则需要满足$J(\Theta)$对$\Theta$的导数等于0向量，即
$$\bigtriangledown_{\Theta}J(\Theta)=\vec{0}$$
由$J(\Theta)$的计算公式可知

$$J(\Theta)=\frac{1}{2}(X\Theta-Y)^T(X\Theta-Y)=\frac{1}{2}(\Theta^TX^T-Y^T)(X\Theta-Y)=\frac{1}{2}(\Theta^TX^TX\Theta-\Theta^TX^TY-Y^TX\Theta+Y^TY)$$

$J(\Theta)$是一个实数，所以

$$\bigtriangledown_{\Theta}J(\Theta)=\bigtriangledown_{\Theta}trJ(\Theta)=\frac{1}{2}(\bigtriangledown_{\Theta}tr\Theta^TX^TX\Theta-\bigtriangledown_{\Theta}tr\Theta^TX^TY-\bigtriangledown_{\Theta}trY^TX\Theta+\bigtriangledown_{\Theta}trY^TY)$$

逐项分析：
第一项

$$\bigtriangledown_{\Theta}tr\Theta^TX^TX\Theta=\bigtriangledown_{\Theta}tr\Theta\Theta^TX^TX=\bigtriangledown_{\Theta}tr\Theta I\Theta^TX^TX=X^TX\Theta+X^TX\Theta$$

上述推导最后一步把$\Theta$看作$A$，$I$看作$B$,$X^TX$看作$C$,应用上一小结中最后一条定理，$I$为单位矩阵
第二项
由于\Theta^TX^TY是一个实数，所以

$$\bigtriangledown_{\Theta}tr\Theta^TX^TY=\bigtriangledown_{\Theta}trY^TX\Theta=(Y^TX)T=X^TY$$

上述推导中倒数第二步是把$Y^TX$看作$B$，$\Theta$看作$A$，利用上一小节中的定理三得到的。
第三项与第二项相同，第四项与$\Theta$无关，因此求导后为0,可以忽略。
最终可得

$$\bigtriangledown_{\Theta}J(\Theta)=\frac{1}{2}(X^TX\Theta+X^TX\Theta-X^TY-X^TY)=0$$

$$\Rightarrow X^TX\Theta=X^TY$$

$$\Rightarrow \Theta=(X^TX)^{-1}X^TY$$

**Tips**

如果$X^TX$不可逆，可以用伪逆的方式计算，但一般情况下这种情况是因为特征选取的时候有依赖造成的。

{% include JB/setup %}

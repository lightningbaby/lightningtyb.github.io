---
title: ML1-Perceptron
date: 2018-04-01 18:20:02
tags: machine learing
mathjax: true
---
# 感知机
### 什么是感知机
基于误分类的损失函数，利用梯度下降法对损失函数进行极小化，求得感知机模型。
<!-- more -->
分为原始形式和对偶形式。
感知机是1957年由Rosenblatt提出，是神经网络和SVM的基础。

### 1. 感知机模型
定义（是什么我已经忘记了）
假设输入空间（特征空间）是$ X \subseteq R^n  $,输出空间是$Y = \{ -1,+1 \} $，输入$y\in Y$表示实例的类别。由输入空间到输出空间的如下函数
                               \begin{equation}f(x)=sign (w\cdot x + b)\end{equation}
称为感知机。其中，w和b为感知机模型参数，$w \in R^n$叫做权值或权值向量，$b \in R$叫做偏置（bias），$w \cdot x$表示w和x的內积。sign是符号函数，即
\begin{equation}
f(x)=
\begin{cases}
+1& \text{$x \geq 0$}\\
-1& \text{x < 0}
\end{cases}
\end{equation}
感知机是一种线性分类模型，属于判别模型。感知机模型的假设空间是定义在特征空间中的所有线性分类模型（linear classification model）或线性分类器（linear classifier），即函数集合$\{f|f(x)=w\cdot x+b\}$.
	感知机的几何解释：线性方程
	\begin{equation}
	w\cdot x+b=0
\end{equation}
对应于特征空间$R^n$中的一个超平面S，w是S的法向量，b对应于该超平面的截距。S将该特征空间分为两部分，位于两部分的点分别被分为正、负两类。称超平面S为分离超平面（separating hyperplane）.
	感知机学习，由训练数据集（实例的特征向量和类别）
	\begin{equation}
	T=\{(x_1,y_1),(x_2,y_2),...(x_N,y_N)\}
	\end{equation}
其中，$x_i \in X=R^n,y_i \in Y=\{+1,-1\},i=1,2,...N$,求得感知机模型，即求模型参数w，b。

### 2. 学习策略
定义：数据集的线性可分性，给定一个数据集，同上，如果存在某个超平面S
\begin{equation}
w\cdot x+b=0
\end{equation}
能够将数据集的正实例点和负实例点完全正确的划分到超平面的两侧，即对所有$y_i=+1的实例i，有w\cdot w_i+b>0$,对所有$y_i=-1的实例i，有w\cdot w_i+b<0$，则该数据集T为线性可分数据集（linearly separable data set）；否则，称数据集T线性不可分。

##### 感知机学习策略
假设数据集是线性可分的，感知机学习的目标是能将该数据集完全正确分离的超平面，即需要去顶参数w，b，需要确定学习策略，即定义损失函数并最小化。
损失函数可以是误分类点的总数，但这样不是w,b的连续可导函数，难优化。
感知机采用的是误分类点到超平面S的总距离。
输入空间$R^n$中任一点$x_o$到超平面S的距离：
\begin{equation}
\frac{1}{||w||} |w\cdot x_0+b|
\end{equation}
这里，$||w||$是$w的L2$范数（欧式距离是一种$L2$范数）。
	对于，误分类的数据$(x_i,y_i)$来说，
\begin{equation}
	-y_i(w\cdot x_i +b)>0
\end{equation}
成立。因此误分类点$x_i$到超平面S的距离是
\begin{equation}
-\frac{1}{||w||} （w\cdot x_i+b）
\end{equation}
对于误分类集合M，所有误分类点到超平面S的总距离为
\begin{equation*}
-\frac{1}{||w||}\sum_{x_i\in M } y_i(w\cdot x_i+b)
\end{equation*}
不考虑范数，可以得到感知机$sign(w\cdot x+b)$学习的损失函数（是感知机的经验风险函数）:
\begin{equation}
L(w,b)=-\sum_{x_i\in M } y_i(w\cdot x_i+b)
\end{equation} 
一个特定的样本点的损失函数：在误分类时是参数w,b的线性函数，在正确分类是，损失函数的值为0。因此，给定训练数据集T，损失函数$L(w,b)是w,b$的连续可导函数。
感知机学习的策略，即在假设空间中选取使损失函数最小的模型参数$w,b$，即感知机模型。
#### 3. 学习算法
感知机学习问题转化为求解损失函数式的最优化问题，最优化的方法是随机梯度下降法。
##### 3.1 原始形式
给定一个数据集（同前面的描述），求参数$w,b$，使其为以下损失函数极小化问题的解
\begin{equation}
\min_{w,b}L(w,b)=-\sum_{x_i\in M } y_i(w\cdot x_i+b)
\end{equation} 
学习算法具体采用随机梯度下降（stochastic gradient descent）。首先，任意选取一个超平面$w_0,b_0$，然后用随机选取一个误分类点使其梯度下降。
\begin{equation}
\nabla_wL(w,b)=-\sum_{x_i\in M}y_ix_i
\end{equation}
\begin{equation}
\nabla_bL(w,b)=-\sum_{x_i\in M}y_i
\end{equation}
假设M固定，损失函数$L(w,b)$的梯度有由上述两式给出。
随机选取一个误分类点$(x_i,y_i)$，对$w,b$进行更新：
\begin{equation}
w\gets w+\eta y_ix_i
\end{equation}
\begin{equation}
b\gets w+\eta y_i
\end{equation}
$\eta（0\le \eta \le 1）$是步长，即学习率（learning rate）。通过迭代不断减小损失函数，直到为0.
算法如下：
输入：训练数据集T，学习率
输出：$w,b$；感知机模型$f(x)=sign (w\cdot x + b)$
（1）选取初值$w_0,b_0$
（2）在训练集中选取数据$(x_i,y_i)$
（3）如果$y_i(w\cdot x_i+b)\le 0$
\begin{equation}
w\gets w+\eta y_ix_i
\end{equation}
\begin{equation}
b\gets w+\eta y_i
\end{equation}
（4）转至步骤2，直至训练集中么有误分类点。
感知机学习算法由于采用不同的初值或选取不同的误分类点，解可以不同。
##### 3.2 算法收敛性证明
对于线性可分数据集感知机学习算法原始形式收敛，即经过有限次的迭代，可以得到一个能将训练集完全正确划分的超平面和感知机模型。
感知机算法在训练数据集上的误分类次数k满足不等式：
\begin{equation}
k\le\lgroup \frac{R}{\gamma}\rgroup^2
\end{equation}

##### 3.3 对偶形式
将$w,b$表示为实例$x_i和标记y_i$的线性组合形式，通过求解洗漱从而求得$w和b$。假设初始值$w_0,b_0$均为0。对误分类点$(x_i,y_i)$通过
\begin{equation}
w\gets w+\eta y_ix_i
\end{equation}
\begin{equation}
b\gets w+\eta y_i
\end{equation}
不断修改$w,b$，设修改n次，则$w,b$关于$(x_i,y_i)$的增量分别为$\alpha_iy_ix_i和\alpha_iy_i，\alpha_i=n_i\eta$，最后学习都的$w,b$可以分别表示为
\begin{equation}
w=\sum_{i=1}^N\alpha_iy_ix_i 
\end{equation}
\begin{equation}
b\sum_{i=1}^N\alpha_iy_i
\end{equation}
这里，$\alpha_i\ge0,i=1,2,...N,当\eta=1时，表示第i$个实例点由于误分而进行更新的次数。更新次数越多，距离分离超平面越近，就越难正确分类。这样的实例对学习结果影响最大。
##### 算法
输入：线性可分数据集T，学习率
输出：$\alpha,b$，感知机模型$f(x)=sign\lgroup \sum_{j=1}^N\alpha_iy_ix_i +b \rgroup，其中\alpha=(\alpha_1,\alpha_2,...\alpha_N)^T$
（1）$\alpha \gets 0,b\gets 0$
（2）在训练集中选取数据$(x_i,y_i)$
（3）如果$y_i\lgroup \sum_{j=1}^N\alpha_iy_ix_i +b \rgroup\le0$
\begin{equation}
\alpha_i\gets\alpha_i+\eta
\end{equation}
\begin{equation}
b\gets b+\eta
\end{equation}
（4）转至（2）直至没有误分类数据
对偶实例中训练实例仅以內积的形式出现，可以预先将训练集中实例间的內积计算出来并以矩阵形式存储，即Gram 矩阵。
同样，对偶形式也是收敛的，存在多个解。
### 4. 习题
（1）验证感知机为什么不能表示异或
虽然通过画图或者异或表，可以看出，但还想到怎样形式化的证明。待续
（2）构建从训练数据集求解感知机模型的例子
（3）证明一下定理：样本集线性可分的充分必要条件是正实例点集所构成的凸壳，与负实例点集所构成的凹壳互不相交。


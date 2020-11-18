title: SVM
author: hero576
tags:
  - 机器学习
categories:
  - 机器学习
date: 2018-05-10 15:50:00
---
> 
<!-- more -->


## 支持向量机
### 间隔与支持间隔
分类学习的最基本想法就是基于训练集D在样本空间中找到一个划分超平面，将不同类别的样本分开。
能将训练样本划分开的平面可能有很多个，选择位于两类训练样本正中间的划分超平面，原因是这个超平面的分类结果最鲁棒，泛化能力最强。
在样本空间中，划分超平面可通过以下线性方程来描述 
$$ω^Tx+b=0$$
样本空间中任意点 x 到超平面 (w,b) 的距离可以写成 
$$r=\frac{|w^Tx+b|}{||w||}$$
假设超平面能够正确分类样本，则可以通过对 ω 缩放可以使得下式成立 
\begin{cases}
|w^Tx+b|>=1,y_i=+1
|w^Tx+b|<=-1,y_i=-1
\end{cases}
距离超平面最近的几个样本点使得上式等号成立，称作“支持向量”。两个异类支持向量到超平面的距离之和称为“间隔”，$γ=\frac{2}{||w||}$。所有位于最大边界上的点称作支持向量。撑起了边界的宽度，其中$||w||$是向量的范数（norm）：
$$if W = {w_1,w_2,....w_n}then\sqrt{W·W}=\sqrt{w_1^2,w_2^2,....w_n^2}$$
![upload successful](/images/pasted-27.png)
距离是x和x’在w上的投影：
![upload successful](/images/pasted-28.png)
欲最大化间隔，等价于最小化$||w||^2$ , 这就是支持向量机的基本型。 
$$\min{w,b} \frac{1}{2} ||w||^2$$
$$s.t. y_i(w^Tx_i+b)≥1，i=1,2,...,m$$
![upload successful](/images/pasted-29.png)

## 对偶问题
上述问题是一个凸二次规划问题，能直接用现成的优化计算包求解。但是通过拉格朗日乘子法变换到对偶变量的优化问题之后，可以找到一种更加有效的方法来进行求解。
原问题的拉格朗日函数为 
$$L(w,b,α)=\frac{1}{2}||w||^2+\sum{i=1}^m α_i(1-y_i(w^Tx_i+b))$$
求偏导为零可以得到 
$$w=\sum{i=1}^m α_iy_ix_i$$
$$0=\sum{i=1}^mα_iy_i$$
对偶问题，将最大最小互换： 
$$\min{w,b}\max{α}L(w,b,α)->\max{α}\min{w,b}L(w,b,α)$$
$$\max{α} \sum{i=1}{m}α_i-\frac{1}{2}\sum{i=1}^m\sum{j=1}^mα_iα_jy_iy_jx_i^Tx_j{}$$
$$s.t. \sum{i=1}^mα_iy_i=0,α_i≥0，i=1,2,...,m$$
于是可以得到模型为 
$$f(x)=w^Tx+b=\sum{i=1}^mα_iy_ix_i^Tx+b$$
上述过程需要满足KKT(Karush-Kuhn-Tucker)条件，即 
\begin{cases}
α_i≥0
y_if(x_i)-1≥0
α_i(y_if(x_i)-1)=0
\end{cases}
对任意训练样本 $(x_i,y_i)$ ，总有 $α_i=0$ 或 $y_if(x_i)=1$ 。因此训练完成后，大部分的样本都不需要保留，最终模型仅与支持向量有关。
如果用二次规划算法求解对偶问题，则问题的规模正比于训练样本数，这会在实际任务中造成很大开销，为此提出SMO(Sequential Minimal Optimization)算法。

#### SMO算法
SMO将比较大的问题拆解为比较小的问题，多个变量不好求，先求两个。
步骤：不断执行以下两个步骤直到收敛 
1、选取一对需要更新的变量 αi 和 αj 
2、固定 αi 和 αj 以外的参数，求解对偶问题更新后的 αi 和 αj 
只要选取的 αi 和 αj 中有一个不满足KKT条件， 目标函数就会在迭代后减小。KKT条件违背的程度越大，变量更新后可能导致的目标函数值减幅越大。
使选取的两变量所对应样本之间的间隔最大（两个变量有很大的差别，对它们进行更新会带给目标函数值更大的变化）。
### 核函数
原始样本空间线性不可分：将样本从原始空间映射到一个更高维的特征空间，使得样本在这个特征空间内线性可分。如果原始空间是有限维，那么一定存在一个高维特征空间使样本可分。
模型变成：$ω^Tϕ(x)+b=0 $
$$\min{w,b}\frac{1}{2}||w||^2$$
$$s.t. y_i(w^Tϕ(x_i)+b)≥1，i=1,2,...m$$
对偶问题为：
$$\max{α} \sum{i=1}{m}α_i-\frac{1}{2}\sum{i=1}^m\sum{j=1}^mα_iα_jy_iy_jx_i^Tx_j{}$$
$$s.t. \sum{i=1}^mα_iy_i=0,α_i≥0，i=1,2,...,m$$
由于特征空间维数可能很高，直接计算 $ϕ(x_i)^Tϕ(x_j)$通常是困难的。设想函数$k(x_i,x_j)=ϕ(x_i)Tϕ(x_j)$,$x_ixi$与$x_jxj$在特征空间的内积**等于**它们在原始样本空间中通过核函数计算的结果。
核函数选择成为支持向量机的最大变数，若核函数选择不合适，则意味着将样本映射到了一个不合适的特征空间，很可能导致性能不佳。比如图像分类，一般使用高斯径向基核函数，因为需要超平面要非常的平滑。  
常用核函数： 
![upload successful](/images/pasted-30.png)


## 软间隔与正则化
现实任务中往往很难确定合适的核函数使得训练样本在特征空间中线性可分，即便线性可分，也很难判定这个结果不是由于过拟合造成的。
缓解这个问题的一个方法是允许支持向量机在一些样本上出错，引入“软间隔”概念。允许某些样本不满足约束 $y_i(ω^Txi+b)≥1 $
优化目标可写成 
$$\min{w,b}\frac{1}{2}||w||^2+C\sum{i=1}^mξ_i$$
其中 l0/1 是“0/1损失函数”
 ![upload successful](/images/pasted-11.png)
l0/1 非凸、非连续、数学性质不好，使得上式难以求解，因此人们用其他一些函数来代替它，称为“代替函数”。
常用的代替函数：
![upload successful](/images/pasted-21.png)
常用的软间隔支持向量机：
![upload successful](/images/pasted-31.png)
与硬间隔支持向量机相似，软间隔支持向量机也是一个二次规划问题，可以通过拉格朗日乘子法得到拉格朗日函数：
![upload successful](/images/pasted-32.png)
求偏导为零可以得到
![upload successful](/images/pasted-33.png)
对偶问题为 
![upload successful](/images/pasted-34.png)
上述过程需要满足KKT(Karush-Kuhn-Tucker)条件，即 
![upload successful](/images/pasted-35.png)
实际上支持向量机和对率回归的优化目标相近，通常情况下他们的性能相当。对率回归的优势主要对于其输出具有自然的概率意义，即在给出预测标记的同时也给了概率，而支持向量机的输出不具有概率意义，欲得到概率需要进行特殊处理；此外，对率回归能够直接用于多分类任务，支持向量机为此需要进行推广。另一方面，可以看出hinge损失函数有一块平摊的零区域，这使得支持向量机的解具有稀疏性，而对率损失是光滑的而单调递减函数，不能导出类似支持向量的概念。因此对率回归的解依赖于更多的训练样本，其预测开销大。
```python
import numpy as np
import pylab as pl
np.random.seed(0)
X=np.r_[np.random.randn(20,2)-[2,2],np.random.randn(20,2)+[2,2]]
Y=[0]*20+[1]*20
clf=svm.SVC(kernel='linear')
clf.fit(X,Y)
w=clf.coef_[0]
a=-w[0]/w[1]
xx=np.linspace(-5,5)
yy=a*xx-(clf.intercept_[0])/w[1]
b=clf.support_vectors_[0]
yy_down=a*xx+b[1]-a*b[0]
b=clf.support_vectors_[-1]
yy_up=a*xx+b[1]-a*b[0]
pl.plot(xx,yy,'k-')
pl.plot(xx,yy_down,'k--')
pl.plot(xx,yy_up,'k--')
pl.scatter(clf.support_vectors_[:,0],clf.support_vectors_[:,1])
pl.scatter(X[:,0],X[:,1],c=Y,cmap=pl.cm.Paired)
pl.show()
```



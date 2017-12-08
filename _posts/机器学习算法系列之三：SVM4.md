---
title: 机器学习算法系列之三：SVM（4）
date: 2017-07-01 21:54:16
tags: [机器学习, 算法, SVM, 支持向量机]
categories: [机器学习,  支持向量机] 
comments: true
toc: true
---
<img src="机器学习算法系列之三：SVM4/SVM4首图.png" width="350" height="250" />
><font color=#0000FF face="微软雅黑" size=4>“你听过的最有哲理，也是你最喜欢的一句话是什么？”，他问。</br>“我也不知道...”，她淡淡的说道。</font>
***

## 一、对偶算法
&emsp;&emsp;<font color=#000000 size=5>**在**</font>本系列的第（3）篇中，我们证明了分割超平面的存在性和唯一性，这章我们来看看如何求解这个分割超平面。我们先来看看要求解的最优化问题，如下式：

$$\begin{cases}
\min \limits\_{w,b}{\frac{1}{2} {||w||}}^2\\\\
y_i \lgroup {\vec w} · \vec x_i + {b} \rgroup - 1  \geq 0, \ \ i = 1,2,···,N
\end{cases}
\tag{\*2 - 14}
$$

&emsp;&emsp;为了求解该凸优化问题，先构造出其广义拉格朗日函数（见本博客“拉格朗日对偶问题”专栏），如下式1-1：

$$
L(x,\alpha,\beta) = {\frac{1}{2} {||w||}}^2 - { \sum\_{i=1}^N \alpha\_i  y\_i (\vec w · \vec x\_i) } +  { \sum\_{i=1}^N \alpha\_i }
\tag{1 - 5}
$$
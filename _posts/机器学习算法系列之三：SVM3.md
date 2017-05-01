---
title: 机器学习算法系列之三：SVM（3）
date: 2017-04-30 14:44:26
tags: 机器学习 算法 SVM 支持向量机
categories: [机器学习,  支持向量机] 
comments: true
toc: true
---
<img src="机器学习算法系列之三：SVM3/SVM3首图.png" width="500" height="800" />
><font color=#0000FF face="微软雅黑" size=4>The road to dreams is not only lonely, but also rugged！</font>
***   

## 一、理论证明  
&emsp;&emsp;<font color=#000000 size=5>**在**</font>本系列的前两章主要是对SVM的一些相关概念做了介绍，对于只想简单了解SVM原理的来说，已经足够了。但对于想进一步深入理解SVM的来说，收拾好你的行囊，整理好你的思绪，踏上新的征程吧，前面的路途不但遥远，而且崎岖。
&emsp;&emsp;本章内容主要是SVM的理论证明部分，<font color=#FF0000 face="微软雅黑" size=4>前方各种公式定理高能预警！</font>**以下内容可能会引起不适，主要表现为犯困、头晕、有暴力冲动，请自行选择是否继续。**
<!-- more -->

### 1.1 支持向量  
&emsp;&emsp;所谓支持向量（Support Vector），是指训练数据集的样本点（线性可分）中与分离超平面距离最近的样本点。在[机器学习算法系列之三：SVM（2）](https://flat2010.github.io/2017/02/17/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AE%97%E6%B3%95%E7%B3%BB%E5%88%97%E4%B9%8B%E4%B8%89%EF%BC%9ASVM2/)中我们讲到了分割超平面求解的等价问题，以归一化（函数间隔$\hat{\gamma} = 1$）后的问题来说，求解出的超平面必然使得所有点都满足：

$$
y_i(\vec w · x_i + b) \geq 1
\tag{1 - 1}
$$

&emsp;&emsp;即所有点到分割超平面的距离都不小于1，那么**支持向量就是指这些到超平面的距离刚好等于1的点。**其实决定参数$\vec w、b$的，正是这些点。这也是支持向量机这个称谓的由来，即由一组支持向量控制决定的分类模型。

### 1.2 缓冲带
&emsp;&emsp;请注意，缓冲带这个词并非术语，是我根据它的作用起的一个名字，方便大家理解它的功能。它的专业术语称作**margin**，是由过支持向量并与分割超平面平行的两个平面所围起来的部分，由于支持向量到分割超平面的距离均为1，所以**分割超平面位与这两个平面的中间位置**，如下图所示：
![image](机器学习算法系列之三：SVM3/缓冲区等示意图.PNG)
<div align='center'>图1-1　　缓冲区、支持向量等示意图</div> 

&emsp;&emsp;如图上所示，L1、L2上的点就是**支持向量**，黄色粗直线为分割超平面（由$\vec w、b$决定），L1、L2之间的距离就是**margin = $\frac{2}{||w||}$**，L1、L2之间的区域即为缓冲带（缓冲区），L1、L2到分割超平面的距离均为**几何距离 = $\frac{1}{||w||}$**。
&emsp;&emsp;之所以把L1和L2之间的区域称为缓冲带，是因为任意一边的样本点发生抖动时，都不会立即越过分割超平面被误判为另一种类型的样本，这个安全距离就是$\frac{1}{||w||}$，缓冲带的存在确保了SVM的泛化能力和抗噪能力。
&emsp;&emsp;同时由上图可知，**分割超平面只与支持向量有关**，去掉或者移动其它非支持向量（不越过支持向量），并不会影响最终的解。**SVM分类模型训练的本质就是找到这些支持向量。**

### 1.3 存在性和唯一性
&emsp;&emsp;给定数据集T（**线性可分**），则有且仅有一个如上所述的分割超平面存在，使得几何间隔$\gamma$最大。即[机器学习算法系列之三：SVM（2）](https://flat2010.github.io/2017/02/17/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AE%97%E6%B3%95%E7%B3%BB%E5%88%97%E4%B9%8B%E4%B8%89%EF%BC%9ASVM2/)中的式**2-14**有且仅有一个解，现在分别从存在性和唯一性方面进行证明。为了方便阅读，我把前面提到的公式2-14贴在这里。

$$\begin{cases}
\min \limits\_{w,b}{\frac{1}{2} {||w||}}^2\\\\
y_i \lgroup {\vec w} · \vec x_i + {b} \rgroup - 1  \geq 0, \ \ i = 1,2,···,N
\end{cases}
\tag{\*2 - 14}
$$

#### 1.3.1 存在性
&emsp;&emsp;存在性的证明相对比较简单。因为给定的数据集$T$线性可分（非线性可分其实也有解，后面会讲），所以式2-14一定有解。对式子中的最小化问题有：

$$
\min \limits\_{w,b}{\frac{1}{2} {||w||}}^2 = 
\min \limits\_{w,b}{\frac{1}{2} \sqrt{w_1^2 + w_2^2 + ··· + w_n^2}^2}
$$

&emsp;&emsp;设$w_i^2 \geq w_k^2，i \neq k，i = 1，2，···，n$，则有：

$$
\min \limits\_{w,b}{\frac{1}{2} {||w||}}^2 \geq
{\frac{1}{2} \sqrt{n·w_k^2}^2} \geq
{\frac{n}{2}} w_k^2
$$

&emsp;&emsp;又因为$n、w_k$都是有界实数，所以必然有：

$$
\min \limits\_{w,b}{\frac{1}{2} {||w||}}^2 \geq
K，K \in R^+且K \nrightarrow \infty
\tag{1 - 2}
$$

&emsp;&emsp;**即$\min \limits\_{w,b}{\frac{1}{2} {||w||}}^2$必然有解。**记最优解为$(w^\*, b^\*)$，可知$w^\* \neq 0$，注意这点很重要，因为$w^\* = 0$就会导致$||w|| = 0$，也即几何间隔不存在。

#### 1.3.2 唯一性
&emsp;&emsp;对唯一性的证明，我们用反证法。设2-14的解存在且不唯一，记$(\vec w\_1^\*，b\_1^\*)、(\vec w\_2^\*，b\_2^\*)$分别为式2-14的两个最优解。
&emsp;&emsp;因为$(\vec w\_1^\*，b\_1^\*)、(\vec w\_2^\*，b\_2^\*)$均为最优解，则$(\vec w\_1^\*，b\_1^\*)、(\vec w\_2^\*，b\_2^\*)$必然均满足目标函数：

$$
\min \limits\_{w,b}{\frac{1}{2} {||w||}}^2
$$

&emsp;&emsp;因此必有：

$$
\min \limits\_{w,b}{\frac{1}{2} {||w||}}^2 = 
{\frac{1}{2} {||w\_1^\*||}}^2 = 
{\frac{1}{2} {||w\_2^\*||}}^2 = 
m，m \in R^+，常数
$$

&emsp;&emsp;所以必有：

$$
||w\_1^\*|| = ||w\_2^\*|| = c，c \in R^+，常数
\tag{1 - 3}
$$

&emsp;&emsp;取$\vec w^\* = \frac{\vec w_1^\* + \vec w_2^\*}{2}，b = \frac{b_1^\* + b_2^\*}{2}$，则：

$$
\begin{split}
\min \limits\_{w^\*,b^\*}{\frac{1}{2} {||w^\*||^2 } }
& = 
\min \limits\_{w^\*,b^\*}{\frac{1}{2} {|| \frac{w\_1^\* + w\_2^\*}{2} ||^2}  }\\\\
& =  
\min \limits\_{w^\*,b^\*}{\frac{1}{4} {|| {w\_1^\* + w\_2^\*} ||^2}  }\\\\
\end{split}
$$

&emsp;&emsp;由矢量合成法则可知：

$$
||\vec w\_1^\* + \vec w\_2^\*|| = ||\vec w^\*|| \leq ||\vec w\_1^\*|| + ||\vec w\_2^\*||
\tag{1 - 4}
$$
&emsp;&emsp;**当且仅当$\vec w_1、\vec w_2$同向即$\vec w_1 = \lambda · \vec w_2，\lambda \in R^+$时上式取等号。**
&emsp;&emsp;由此有：

$$
\begin{split}
\min \limits\_{w^\*,b^\*}{\frac{1}{2} {||w^\*||^2 } }
& =  
\min \limits\_{w^\*,b^\*}{\frac{1}{8} {|| {w\_1^\* + w\_2^\*} ||^2}  }\\\\
& \leq
\min \limits\_{w^\*,b^\*}{\frac{1}{8} {(|| {w\_1^\*|| + ||w\_2^\*} ||)^2}  } \\\\
&=
\min \limits\_{w^\*,b^\*}{\frac{1}{8} { ( ||w\_1^\*||^2 + ||w\_2^\* ||^2+ 2||w_1^\*||·||w_2^\*|| )} }
\end{split}
$$

&emsp;&emsp;$\because$

$$
||w\_1^\*|| = ||w\_2^\*||
$$

&emsp;&emsp;$\therefore$

$$\begin{split}
\min \limits\_{w^\*,b^\*}{\frac{1}{2} {||w^\*||^2 } } 
& \leq
\min \limits\_{w^\*,b^\*}{\frac{1}{8} (||w\_1^\*||^2 + ||w\_1^\*||^2 + 2||w\_1^\*||·||w\_1^\*||)} \\\\
&=
\min \limits\_{w^\*,b^\*}{\frac{1}{8} · 4 · ||w\_1^\*||^2} \\\\
& =
\min \limits\_{w^\*,b^\*}{ \frac{1}{2} {||w\_1^\*||^2 } } \\\\
& = 
m
\end{split}
$$

&emsp;&emsp;<font color=#FF0000 face="微软雅黑" size=4>即$\vec w^\* = \frac {\vec w\_1^\* + \vec w\_2^\*}{2}$也是使得几何间隔最大的分割超平面。</font>
&emsp;&emsp;对不等式约束条件，有：
$$\begin{split}
y\_i \lgroup {\vec w^\*} · \vec x_i + {b^\*} \rgroup - 1 &=
y\_i \lgroup { \frac{\vec w\_1^\* + \vec w_2^\*}{2}} · \vec x_i + \frac{b\_1^\* + b\_2^\*}{2} \rgroup - 1 \\\\
&=
\frac{1}{2} y\_i \lgroup { \vec w\_1^\*} · \vec x\_i + b\_1^\* + {\vec w_2^\*} + b\_2^\* \rgroup - 1 \\\\
&=
\frac{1}{2} y\_i \lgroup { \vec w\_1^\*} · \vec x\_i + b\_1^\* \rgroup + 
\frac{1}{2} y\_i \lgroup { \vec w\_2^\*} · \vec x\_i + b\_2^\* \rgroup - 1
\end{split}
$$
&emsp;&emsp;又因为$\vec w_1^\*、\vec w_2^\*$满足：

$$\begin{cases}
y\_i \lgroup {\vec w\_1^\*} · \vec x\_i + {b^\*} \rgroup - 1  \geq 0, \ \ i = 1,2,···,N \\\\
y\_i \lgroup {\vec w\_2^\*} · \vec x\_i + {b^\*} \rgroup - 1  \geq 0, \ \ i = 1,2,···,N
\end{cases}
$$

&emsp;&emsp;所以有：

$$\begin{split}
y\_i \lgroup {\vec w^\*} · \vec x\_i + {b^\*} \rgroup - 1 &\geq
\frac{1}{2} · 1 + 
\frac{1}{2} · 1 - 1 = 
1 - 1 =
0
\end{split}
$$

&emsp;&emsp;<font color=#0000FF face="微软雅黑" size=4>即$\vec w^\* = \frac {\vec w\_1^\* + \vec w\_2^\*}{2}、b^\* = \frac{b_1^\* + b_2^\*}{2}$同时还满足不等式的约束条件。</font>
&emsp;&emsp;<font color=#FF0000 face="微软雅黑" size=4>综上两点可知$\vec w^\* = \frac {\vec w\_1^\* + \vec w\_2^\*}{2}、b^\* = \frac{b_1^\* + b_2^\*}{2}$也是最优解！</font>
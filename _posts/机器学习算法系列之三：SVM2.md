---
title: 机器学习算法系列之三：SVM（2）
date: 2017-02-17 01:41:34
tags: [机器学习, 算法, SVM, 支持向量机]
categories: [机器学习] 
comments: true
toc: true
---
<img src="机器学习算法系列之三：SVM2/SVM2首图.PNG" width="350" height="250" />
><font color=#0000FF face="微软雅黑" size=4>Hyper plane!</font>
***

## 一、SVM  
&emsp;&emsp;  &emsp;&emsp;<font color=#000000 size=5>**在**</font>[机器学习算法系列之三：SVM（1）](https://flat2010.github.io/2017/01/29/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AE%97%E6%B3%95%E7%B3%BB%E5%88%97%E4%B9%8B%E4%B8%89%EF%BC%9ASVM1/)中我们提到了SVM，但并没有给出它的定义，也没有做过多的介绍。现在，是时候隆重的介绍我们本系列的主角了。在这小节里面，下面就从SVM的定义，用途，优缺点来这些方面来进行叙述。  
<!-- more -->

### 1.1 定义  
&emsp;&emsp;老规矩，先上维基百科的定义，如下：  

<blockquote2>In machine learning, support vector machines (SVMs, also support vector networks) are <font color=#FF0000 size=3>supervised learning models</font> with associated learning algorithms that analyze data <font color=#FF0000 size=3>used for classification and regression analysis.</font> Given a set of training examples, each marked as belonging to one or the other of two categories, an SVM training algorithm builds a model that assigns new examples to one category or the other, making it a <font color=#FF0000 size=3>non-probabilistic binary linear classifier.</font> An SVM model is a representation of the examples as points in space, <font color=#FF0000 size=3>mapped</font> so that the examples of the separate categories are divided by a clear <font color=#FF0000 size=4>gap</font> that is as wide as possible. New examples are then mapped into that same space and predicted to belong to a category based on which side of the gap they fall.</blockquote2>  
<blockquote2> More formally, a support vector machine constructs a <font color=#FF0000 size=4>hyperplane or set of hyperplanes in a high- or infinite-dimensional space</font>, which can be used for classification, regression, or other tasks. Intuitively, a good separation is achieved by the hyperplane that has the largest distance to the nearest training-data point of any class (<font color=#FF0000 size=4>so-called functional margin</font>), since in general the larger the margin the lower the generalization error of the classifier.</blockquote2>   

&emsp;&emsp;注意在上述定义中被我标红的关键词，这些是SVM的核心知识点，特别是`gap`、`hyper plane`、`function margin`，当然还有我们后面会讲到的`kernel trick`。对于`kernel trick`，它使得SVM可以被应用于非线性可分的数据集上，并且工作的非常好，大大提高了SVM的通用性，可以说是SVM核心中的核心。关于`kernel trick`的历史，维基百科里面是这样说的：  
<blockquote2> The original SVM algorithm was invented by Vladimir N. Vapnik and Alexey Ya. Chervonenkis in 1963. In 1992, Bernhard E. Boser, Isabelle M. Guyon and Vladimir N. Vapnik suggested a way to create <font color=#FF0000 size=4>nonlinear classifiers</font> by applying the <font color=#FF0000 size=4>kernel trick to maximum-margin hyperplanes.</font> The current standard incarnation (soft margin) was proposed by Corinna Cortes and Vapnik in 1993 and published in 1995.</blockquote2>

### 1.2 用途    
#### 1.2.1 文本/超文本分类  
&emsp;&emsp;SVM在文本（Text）及超文本（Hypertext）分类中非常有用，能够显著减少算法对已标记数据的依赖（即只需少量训练数据）。这主要得益于SVM只需要少数关键的支持向量既能确定一个最优的分类超平面。  
#### 1.2.2 图片分类  
&emsp;&emsp;SVM同样被应用于图像分类，实验数据表明，相比于传统的查询细化方案，SVM仅在三到四轮相关性反馈后就拥有较高的搜索精度。  
#### 1.2.3 手写体识别  
&emsp;&emsp;当然，SVM也可以用于手写体识别。
#### 1.2.4 聚类分析  
&emsp;&emsp;虽然SVM的主要用途是用于分类或者回归，但是它仍然可以被用于聚类分析。当然原生态的SVM并不能直接用于聚类，需要对它进行一定的改进，用于聚类的改进SVM算法称为支持向量聚类（support vector cluster）。
#### 1.2.5 其它领域  
&emsp;&emsp;SVM还被广泛的应用于诸如生物学、化学等领域。在生物学中，SVM已被用于蛋白质分类，并且其分类准确率高达90％。

### 1.3 优缺点    
#### 1.3.1 优点 
&emsp;&emsp;①在小样本数据集上表现优异，特别适合某些特殊（不易采集到数据或数据量本身就不大）应用场景。  
&emsp;&emsp;②适合高维数据。SVM利用内积核函数代替向高维空间的非线性映射。  
&emsp;&emsp;③有非常坚实的理论基础。  
&emsp;&emsp;④鲁棒性较强。SVM对非支持向量不敏感，增、删、改非支持向量不影响模型训练，同时在某些应用中，SVM对核的选取并不敏感。  
&emsp;&emsp;⑤泛化能力强。SVM本身的优化是针对结构风险最小化的，而非经验风险最小化，同时通过函数间隔等概念，降低了对数据规模和数据分布的要求，增强了泛化能力。  

#### 1.3.2 缺点 
&emsp;&emsp;①训练样本较大时，时间及空间复杂度均较高，有时候其计算代价甚至无法接受（SVM采用二次规划来求解支持向量，涉及m阶矩阵的运算问题）。  
&emsp;&emsp;②原始SVM只支持二分类。  
&emsp;&emsp;③理论性太强，涉及知识面太广，不如其它机器学习算法（如KNN、决策树等）好懂。  
&emsp;&emsp;④在高维空间映射中，如何确定核函数，目前尚无合适的方法。  

&emsp;&emsp;<font color=#FF0000 size=3>注：如果现在还不太明白上面提到的这些概念（比如结构风险），没关系，我们会在后面的过程中详细说明，当然也可以先在网上搜索相关资料，补充一点预备知识，这样会更容易的理解本系列教程。</font>
## 二、超平面/临界条件  
&emsp;&emsp;<font color=#000000 size=5>**由**</font>SVM的定义可知，它通常用于分类或回归，并以分类应用最广。上一节我们引出了Sigmoid函数，它可以用于二分类问题，并且提到了所谓的临界条件（`θ = 0`）。凡是`θ > 0`的，划分成一个类，凡是`θ < 0`的划分成另外一个类。那么，给定一组数据（训练数据集），要对其进行分类时，其实就是要找到满足要求的临界条件（该临界条件由权重系数向量**`w`**和参数**`b`**确定）。专业术语称为`Hyperplane（超平面）`，如果你对这个术语感到畏惧或者疑惑的话，就把它理解为一个临界条件吧。它的数学表达式如下图2-1所示：  
![image](机器学习算法系列之三：SVM2/临界条件表达式.png)
<div align='center'>图2-1　　临界条件表达式</div>  

&emsp;&emsp;为什么称它为超平面呢？当其输入参数为2个时，把上图的向量乘积`w·b`展开，可以看出来这是一个直线方程。当输入参数为3个时，展开后就可以看出来这是一个三维坐标中的平面方程。那么输入参数为4、5、6、···时，是什么东西呢？没人知道，因为人类大脑能构建出的图形的最高维度就3维，所以为了方便我们就把它们统称为超平面了。正如本页第一个图所示，图中的蓝色箭头所指的就是我们的超平面，它把红色小圆点和蓝色小三角形分开了，红色小圆点和蓝色小三角形分别代表两个不同的类别（比如阴性和阳性）。  
&emsp;&emsp;<font color=#FF0000 size=3>注：为叙述方便，本文及后续文章可能会交替使用超平面/临界条件这两个词，其本质并没有区别，请注意。</font>
### 2.1 确定临界条件  
&emsp;&emsp;下面我们以特征向量**维度为2**（即输入参数有2两个）的**线性可分**（非线性可分的处理后面会将）**二分类**为例来说明如何确定临界条件。以如下图2-2所示：  
![image](机器学习算法系列之三：SVM2/确定临界条件1.PNG)
<div align='center'>图2-2　　确定临界条件</div>  

&emsp;&emsp;上图中星星和圆点为不同类型的数据点，要在图中找出能把这两类点区分开的直线的话，是非常容易的。比如图中的红色、蓝色、浅绿色线都可以满足要求，显然这样的线是在上图中我们是可以画无的，也就是对于上图的数据，有无数个满足要求的临界条件。  
&emsp;&emsp;那么问题就来了，学挖掘机究竟哪家强？（哪个临界条件才是最好的^--^）  
### 2.2 几何、函数间隔
&emsp;&emsp;从上图2-2可以看出来，离分割面/线越远的点，它属于某个类的确定性越高，离分割面/线越近，就越靠近临界值，其确定性就越低。对位于分割面/线上的点，其不确定性是最大的。因此，最好的超平面应该满足：  
&emsp;&emsp;**①所有（两侧）距离超平面面最近的点到该超平面的距离之和有最大值！**  
&emsp;&emsp;同时，好的超平面还应该满足：  
&emsp;&emsp;**②使得尽可能多的数据点被正确分类！**  
&emsp;&emsp;由此，我们引出一个新的概念——<font color=#FF0000 size=3>`geometrical margin`（几何间隔）</font>。

#### 2.2.1 点到平面距离
&emsp;&emsp;高中时候我们学过关于给定点**(x<sub>0</sub>,y<sub>0</sub>,z<sub>0</sub>)**到平面**`Ax+By+Cz=0`**的距离公式如下图2-3所示：  
![image](机器学习算法系列之三：SVM2/点到平面的距离1.png)
<div align='center'>图2-3　　点到平面的距离公式</div> 

&emsp;&emsp;如果以向量方式来表示（记平面方程法向量为**`w=(A,B,C)`**，点坐标**x=(x<sub>0</sub>,y<sub>0</sub>,z<sub>0</sub>)**，**b=D**），则有如下图2-4所示公式：

![image](机器学习算法系列之三：SVM2/点到平面的距离2.png)
<div align='center'>图2-4　　向量形式点到平面的距离</div> 

#### 2.2.2 几何间隔
&emsp;&emsp;由上述点到平面距离我们分别定义样本点关于超平面的几何间隔、数据集关于超平面的几何间隔分别为：
<font color=#0000CC>
$$
\gamma_i = y_i·\frac{\vec w·\vec x_i + b}{||\vec w||}
= y_i\lgroup\frac{\vec w}{||\vec w||}·\vec x_i + \frac{b}{||\vec w||}\rgroup
\tag{2 - 1}
$$
</font>
<font color=#CC0033>
$$
\gamma = \min \limits_{i=1,···,N} \gamma_i
\tag{2 - 2}
$$
</font>
&emsp;&emsp;注意式子2-1、2-2中的几何间隔没有像点到平面的距离那样取绝对值，而是可正可负。对于给定的数据集，其几何间隔等于所有样本点到超平面的距离的最小值。且当样本被正确分类时，不管是哪一类样本，都有$\gamma_i \ge 0$（因样本实际类别$y_i$和预测类别$\vec w·\vec x_i + b$同号）。
&emsp;&emsp;对二分类问题，如果我们记样本实际类别$y_i$分别取+1（正类）、-1（负类），由式2-1可知，**`几何间隔`**的**几何意义就是区分正向和反向的点到平面的距离。**

#### 2.2.3 函数间隔
&emsp;&emsp;要概略的理解SVM，有几何间隔就足够了，函数间隔没必要再讲，并且其几何意义远不如几何间隔明晰。但是如果要想理解它的整个推导过程，确保对整个理论体系建立一个完整的框架，还要了解函数间隔的概念及定义式（实际上很多教材和网上的资料都是先讲函数间隔，再引申出几何间隔）。
&emsp;&emsp;**函数间隔实际上是在几何间隔的基础上，扩大了超平面法向量模大小（$||\vec w||$）倍数之后的距离，即：**

$$
\hat{\gamma_i} = ||\vec w||·\gamma_i = y_i\lgroup\vec w·x_i + b\rgroup
\tag{2 - 3}
$$

$$\hat{\gamma} = 
\min \limits_{i=1,···,N} \hat{\gamma_i}
\tag{2 - 4}
$$
#### 2.2.4 相互关系
&emsp;&emsp;几何间隔和函数间隔之间的关系式为：
$$
\begin{split}
几何间隔 &= \frac{函数间隔}{超平面法向量模} \\\\
函数间隔 &=几何间隔·超平面法向量模 \\\\
第i个样本的几何间隔 &= \frac{第i个样本的函数间隔}{超平面法向量模}
\end{split}
$$
$$\gamma_i = \frac{\hat{\gamma_i}}{||w||}
\tag{2 - 5}
$$

$$\gamma = \frac{\hat{\gamma}}{||w||}
\tag{2 - 6}
$$
$$\hat{\gamma} = \gamma·||w||
\tag{2 - 7}
$$

### 2.3 间隔最大化
&emsp;&emsp;由上述几何（函数）间隔的定义可知，间隔越大则表示样本中离超平面最近的点到超平面的距离也越大，被划分为对应类的确信程度也越高，如下图所示：
<img src="机器学习算法系列之三：SVM2/间隔最大化示意图.PNG" width="500" height="400" />
<div align='center'>图2-5　　间隔最大化示意图</div> 

#### 2.3.1 硬间隔最大化
&emsp;&emsp;对线性可分数据集，其几何间隔最大的超平面是**唯一存在（第三篇会证明）**的，对这样的间隔取最大化称为**硬间隔最大化**，当数据集为**近似线性可分**时的间隔最大化称为**软间隔最大化**，这个后面会讲。
&emsp;&emsp;由硬间隔最大化的定义有表达式：    

$$
\max \limits\_{w,b}\ \ \gamma = \max \limits\_{w,b}\(\min \limits\_{i=1,···,N} \gamma_i)
\tag{2 - 8}
$$
&emsp;&emsp;再由函数间隔和几何间隔的关系有：

$$
\max \limits\_{w,b}\ \ \gamma = \max \limits\_{w,b}\ \ \frac{\hat{\gamma}}{||w||}
\tag{2 - 9}
$$

&emsp;&emsp;同时，还应满足如下约束条件（任意点到超平面的距离都不小于几何间隔）：

$$
y_i\lgroup\frac{\vec w}{||w||}·\vec x_i + \frac{b}{||w||}\rgroup\geq\gamma, \ \ i = 1,2,···,N
\tag{2 - 10}
$$

&emsp;&emsp;由上可知，硬间隔最大化问题实际上是关于自变量$w、b、x_i$的最优化求解问题，但是样本$x_i$不应作为优化参数存在，因此最终只剩下$w、b$作为优化问题的自变量，即在满足**式2-10**的约束条件的情况下，求出下面函数极值条件对应的$w、b$：    

$$
\begin{split}
\gamma\_{max} &= \max \limits\_{} \lbrace\ {f(w, b)}\ \rbrace \\\\
& = \max \limits\_{w,b}{\frac{\hat{\gamma}}{||w||}} \\\\
& = \max \limits\_{w, b}{\lbrace\min \limits\_{i=1,···,N}  \ \ {y_i\lgroup\frac{\vec w}{||w||}·\vec x_i + \frac{b}{||w||}\rgroup}}\rbrace
\end{split}
\tag{2 - 11}
$$

#### 2.3.2 最优化问题
&emsp;&emsp;大多数教材或者资料在讲到最大化间隔的时候，会直接甩出一个结论：
&emsp;&emsp;**取函数间隔 $\hat{\gamma} = 1$ 既不会影响目标函数最优化问题的求解，还能大大简化问题的分析。**于是原最优化问题等价为求：

$$\begin{cases}
\max \limits\_{w,b}{\frac{1}{||w||}}\\\\
y_i \lgroup {\vec w} · \vec x_i + {b} \rgroup \geq \hat{\gamma}, \ \ i = 1,2,···,N
\end{cases}
\tag{2 - 12}
$$
&emsp;&emsp;至于为什么这样做不会影响最优化结果，几乎所有资料和教材都只是说不会改变几何间隔，也不会改变式2-10的不等式约束，但没有进行证明。要证明对参数的这种改变不会影响训练结果，我们只需要证明将函数间隔$\hat{\gamma}$调整为1时，以下两点成立即可：    
&emsp;&emsp;**1. 最大硬间隔（几何间隔）的值$\gamma$不会发生改变；**
&emsp;&emsp;**2. 最优化问题的约束条件未发生改变；**
&emsp;&emsp;对第一点，假定将$\vec w、b$同时扩大$\lambda（\lambda > 0）$倍，则几何间隔变为：
$$
\begin{split}
\max \limits\_{\lambda \vec w \ , \ \lambda · b}{\gamma} &= 
\max \limits\_{\lambda \vec w \ , \ \lambda · b}{\frac{\hat{\gamma}}{||\lambda · w||}} \\\\
&= \max \limits\_{\lambda \vec w \ , \ \lambda · b}{\frac{\min \limits\_{\lambda \vec w \ , \ \lambda · b}{y_i (\lambda \vec w x\_i + \lambda · b)}}{||\lambda · w||}} \\\\
&= \max \limits\_{\lambda \vec w \ , \ \lambda · b}{\frac{\lambda \min \limits\_{\lambda \vec w \ , \  \lambda · b}{y_i (\vec w x\_i + b)}}{\lambda ||w||}} \\\\
& = \max \limits\_{\lambda \vec w \ , \ \lambda · b}{\frac{\min \limits\_{\lambda \vec w \ , \  \lambda · b}{y_i (\vec w x\_i + b)}}{||w||}} \\\\
& = \max \limits\_{\vec w \ , \ b}{\frac{\min \limits\_{\vec w \ , \  b}{y_i (\vec w x\_i + b)}}{||w||}} \\\\
& = \max \limits\_{\vec w \ , \ b}{\gamma}
\end{split}
$$
&emsp;&emsp;第一条得证！对于第二条，其实在证明第一条的时候已经包含了，这里还是单独写出来吧，方便阅读，权重向量$\vec w$和常数$b$调整后，对训练数据集中的任意样本$(x_i , y_i)$，有：
$$
\begin{split}
y_i \lgroup \frac{\lambda \vec w}{||\lambda w||} · \vec x_i + \frac{\lambda · b}{||\lambda w||} \rgroup &= 
y_i \lgroup \frac{\lambda \vec w}{\lambda ||w||} · \vec x_i + \frac{\lambda · b}{\lambda ||w||} \rgroup \\\\
& = y_i \lgroup \frac{\vec w}{||w||} · \vec x_i + \frac{b}{||w||} \rgroup \\\\
\end{split}
$$
&emsp;&emsp;**即调整后约束条件与原问题约束条件完全一致！**
&emsp;&emsp;$\because$
$$
y_i \lgroup \frac{\vec w}{||w||} · \vec x_i + \frac{b}{||w||} \rgroup \geq \gamma
\ 且 \ \hat{\gamma} = {\gamma} · ||w||
$$
&emsp;&emsp;$\therefore$
$$
y_i \lgroup {\vec w} · \vec x_i + {b} \rgroup \geq 
\gamma · ||w|| = \hat{\gamma}
$$
&emsp;&emsp;第二条亦得证！<font color="red" size=3px>从几何上来理解，权重向量$\vec w$和常数$b$同时乘以系数$\lambda$，原分割超平面并没有改变（参见平面方程的几何意义），自然几何间隔也就没有改变（所有点到分割面的距离都没改变），但是函数间隔却扩大了$\lambda$倍。</font>
&emsp;&emsp;有了上面的结论，考虑将$\vec w$、$b$同时扩大$\lambda =  \frac{1}{\hat{\gamma}}$倍，函数间隔变为：
$$
\begin{split}
\hat{\gamma}' &=  y_i \lgroup {\lambda \vec w} · \vec x_i + {\lambda · b} \rgroup \\\\
& = \lambda · y_i  · \lgroup {\vec w} · \vec x_i + {b} \rgroup \\\\
& = \lambda · \hat{\gamma} \\\\
& = \frac{1}{\hat{\gamma}} · \hat{\gamma} \\\\
& = 1
\end{split}
$$
&emsp;&emsp;即原最优化问题转换为与下式等价的优化问题：

$$\begin{cases}
\max \limits\_{w,b}{\frac{1}{||w||}}\\\\
y_i \lgroup {\vec w} · \vec x_i + {b} \rgroup - 1  \geq 0, \ \ i = 1,2,···,N
\end{cases}
\tag{2 - 13}
$$

#### 2.2.4 线性可分最优化
&emsp;&emsp;为了方便证明，还要做进一步的等价转换，考虑式2-12中的第一个求最大值表达式，它可以等价为求最小值问题即：

$$
\max \limits\_{w,b}{\frac{1}{||w||}} 
\Longleftrightarrow
\min \limits\_{w,b}{\frac{1}{2} {||w||}^2}
$$

&emsp;&emsp;将求最大值问题转换为求最小值问题是为了方便求导（分数的导数书写起来非常麻烦），求最小值问题的系数**$\frac{1}{2}$**则是为了在对权重向量$\vec w$求一阶导数时消除其导数前面的系数（归一化）。进而，原问题等价于如下的线性可分最优化问题：

$$\begin{cases}
\min \limits\_{w,b}{\frac{1}{2} {||w||}}^2\\\\
y_i \lgroup {\vec w} · \vec x_i + {b} \rgroup - 1  \geq 0, \ \ i = 1,2,···,N
\end{cases}
\tag{2 - 14}
$$
&emsp;&emsp;这是一个**凸二次规划/凸优化问题（convex quadratic programming）**，通俗点说，SVM的凸二次优化问题就是要**寻找参数向量$\vec w$和常数b，使得模型不但能正确的把两个类别的数据区分开，还要让这种分类具有最大的确定性**。凸优化问题是一个非常经典的问题（也是机器学习当中最常见的优化问题），但是理解SVM不需要太深入的了解凸二次规划问题，那本章末尾就简单介绍下凸二次规划问题，想了解更多的可以参考本系列相关专题，仅供有兴趣的同学参考。

### 2.4 凸优化问题
#### 2.4.1 定义表达式
&emsp;&emsp;凸优化问题是指如下的约束最优化问题：

$$\begin{cases}
\min \limits\_{w}f(w) \\\\
g_i(w) \leq 0, \ \ i = 1,2,···,k \\\\
h_i(w) = 0, \ \ i = 1,2,···,l
\end{cases}
\tag{2 - 15}
$$

&emsp;&emsp;式中，优化目标函数$f(w)$和约束函数$g_i(w)$要求都是**连续可微的凸函数**，而约束函数$h_i(w)$则是$R^n$上的**仿射函数**。    
#### 2.4.2 凸函数
&emsp;&emsp;凸函数的定义为：对函数$f(x)，x \in R^n$定义域上的任意两点$x \in R^n , y \in R^n$以及$t \in \lbrack 0 , 1 \rbrack$，函数$f(x)$恒有：

$$
f(tx + (1 - t)y) \leq tf(x) + (1 - t)f(y)
\tag{2 - 16}
$$

&emsp;&emsp;其几何意义为函数任意两点连线上的值均大于对应自变量处的值，图示如下：

<img src="机器学习算法系列之三：SVM2/凸函数示意图.png" width="350" height="250" />
<div align='center'>图2-6　　凸函数示意图</div> 

#### 2.4.3 仿射函数
&emsp;&emsp;所谓仿射函数是指满足如下条件的函数：

$$
f(x) = a · x + b \ , \ a \in R^n \ , \ x \in R^n \ , \ b \in R
\tag{2 - 17}
$$

&emsp;&emsp;这里$a、x$既可以是标量，也可以是向量，甚至矩阵。
#### 2.4.4 凸二次规划
&emsp;&emsp;对于式2-15中的凸优化问题，**当$f(x)$为二次函数，$g\_i(w)$是仿射函数时，凸最优化问题即为凸二次规划问题。**
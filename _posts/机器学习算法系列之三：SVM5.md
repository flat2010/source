---
title: 机器学习算法系列之三：SVM（5）
date: 2018-03-31 16:01:10
tags: [机器学习, 算法, SVM, 支持向量机]
categories: [机器学习,  支持向量机] 
comments: true
toc: true
---
<img src="机器学习算法系列之三：SVM5/SVM5首图.png" width="430" height="200" />

><font color=#0000FF face="微软雅黑" size=4>水至清则无鱼！</font>

***

## 一、线性非可分
### 1.1 可分 *VS* 非可分
&emsp;&emsp;前面章节中我们讨论的分割面的求解均是在数据**线性可分**的条件下进行的，与该条件对应的数学表达式为：

$$
y_i \lgroup {\vec w} · \vec x_i + {b} \rgroup - 1  \geq 0, \ \ i = 1,2,···,N
\tag{1 - 1}
$$

&emsp;&emsp;这个限制条件对应的几何含义就是：**所有数据都能被准确的进行分类。**这个条件对我们简化问题，抽象出数学模型非常有用，但是在面对实际业务的时候却显得非常乏力，因为现实生活中我们所处理的数据几乎都是线性不可分甚至非线性的。这个时候如果不对式（1-1）做调整（改动）的话，会造成两方面的影响：

- 条件过于严格导致方程组在实数域无解，即找不到满足要求的分割面；
- 实数域中求出了分割面（训练数据集线性可分），但是模型泛化能力非常差（使用的时候遇到了线性非可分数据）。

&emsp;&emsp;以上问题，用一句古话总结就是：

> 水至清则无鱼 人至察则无徒

#### 1.1.1 相同点
&emsp;&emsp;线性可分和线性不可分二者建立起来的模型均是线性分类模型。
&emsp;&emsp;数学模型、理论基础是一致的，解决的技术方案（均转换为拉格朗日对偶问题解决）也相同。

#### 1.1.2 不同点
&emsp;&emsp;线性可分模型只能解决线性可分的问题，而线性非可分模型则可以解决所有线性问题（包含了线性可分、线性非可分）。
&emsp;&emsp;线性可分是基础，线性非可分是扩展。
&emsp;&emsp;线性可分使用硬间隔作为优化目标，线性非可分使用软间隔（引入了松弛变量、惩罚系数）作为优化目标。

### 1.2 解决之道
#### 1.2.1 松弛变量
&emsp;&emsp;为了让我们学习出的模型能够处理线性非可分数据，同时增强模型的泛化能力，我们就需要适当的放松间隔的限制条件。**允许少部分数据（噪点、离群点）到分割超平面的几何间隔小于1（位于缓冲地带），甚至为负（位于相反的类中）。**
&emsp;&emsp;因此，我们针对训练集中的每一个样本点都引入一个调控参数，根据需要来调节数据点到分割超平面的距离，这个参数称为**松弛变量$\xi\_i \geq 0$**。于是原来的不等式约束变为下式：

$$
y\_i(\vec w \cdot \vec x\_i + b) \geq 1 - \xi\_i
\tag{1 - 2}
$$

#### 1.2.2 惩罚参数
&emsp;&emsp;引入松弛变量之后，线性非可分问题倒是解决了，却又引入了新的问题。松弛变量因为放宽了限制条件（允许某些点到分割超平面的距离小于1），其中一个类就很容易把另一个类的离群点（更靠近相反类）纳入自己的版图（因为这样会使得函数间隔最大，同时也能满足不等式约束条件），这样就会导致误分类。
&emsp;&emsp;为了解决这个问题，我们就需要对松弛变量做一些约束，具体办法是对引入的每一个松弛变量，我们都让它支付一个等值的代价（我姑且把它称作**松弛代价**，注意是一个非官方术语。）即$\xi \_i$，同时引入一个称为**惩罚参数$C（C > 0）$**的约束参数，来调节对于误分类的容忍程度。
&emsp;&emsp;把惩罚参数、松弛代价整合到目标函数后有：

$$
\min \limits\_{w,b,\xi}{\frac{1}{2} {||\vec w||}}^2 + C\sum\_{i=1}^N \xi\_i\\\\
\tag{1 - 3}
$$

&emsp;&emsp;上式中第一项的优化目标是使得间隔尽量最大，而第二项的求和式优化目标则是使得误分类的点数目尽量最少（松弛代价最小）。
&emsp;&emsp;很明显，对于线性非可分数据来说，因为存在数据交叉，所以间隔越大，误分类的点越多。误分类点越少，必然要求间隔越小。两者处于一种**鱼和熊掌不可兼得的**矛盾状态。
&emsp;&emsp;C作为一个调和参数，平衡着这两者的矛盾关系。<font color="red">C越大，误分类的惩罚越强（代价越高，越在意误分类），表示我们更偏向于学习出一个分类准确率更高的模型；C越小，误分类的惩罚越弱（代价越低，越在意），表示我们更偏向于学习出一个泛化能力更强的模型。</font>
&emsp;&emsp;<font color="red">另外需要补充的一点是，惩罚系数C是作为**超参数**存在的，因此没有加到目标函数中。</font>

#### 1.2.3 求解目标
&emsp;&emsp;最后，我们的线性非可分问题要求解的即是如下凸二次规划问题：

$$
\begin{cases}
\min \limits\_{w,b,\xi}{\frac{1}{2} {||\vec w||}}^2 + C\sum\_{i=1}^N \xi\_i, \ C > 0\\\\
\\\\
y_i \lgroup {\vec w} · \vec x_i + {b} \rgroup - (1 - \xi\_i) \geq 0, \ \ i = 1,2,···,N \\\\
\\\\
\xi\_i \geq 0, \ i=1,2,\cdots,N \\\\
\end{cases}
\tag{1 - 4}
$$

#### 1.2.4 软间隔
&emsp;&emsp;我们把引入了松弛变量、惩罚参数之后的间隔，称为**软间隔**，它是相对于之前线性可分问题的**硬间隔**而言的，意思就是该间隔相对来说限制条件没那么严格。
&emsp;&emsp;上式（1-4）对应的问题也称为**软间隔最大化问题**，由软件隔最大化学习出来的SVM我们称为**线性支持向量机**。
&emsp;&emsp;需要补充说明的一点是，由硬间隔最大化学习出来的SVM我们称为**线性可分支持向量机**，显然，**线性支持向量机**包含了**线性可分支持向量机**，是一个更综合性的称谓。

### 1.3 求证之路
&emsp;&emsp;
#### 1.3.1 存在性证明
&emsp;&emsp;关于线性非可分问题的存在性证明与线性可分问题类似，这里就不赘述了。

#### 1.3.2 <a name="singleton_prove"></a>唯一性证明
&emsp;&emsp;关于线性可分问题的唯一性证明，《统计学习方法》一书中一笔带过，并没有给出详细的证明过程，网上也没有找到相应的资料。目前我还没有完全证明出来，后续证明了再来补齐，这里只贴出结论：
- 权重系数向量$\vec w$的值时唯一的；
- 偏置系数**$b$的值可能不唯一，存在于一个区间**。

#### 1.3.3 求解思路
&emsp;&emsp;与线性可分问题的求解思路一致，我们仍然要先利用KKT条件将原始问题转换为等价的对偶问题，然后再对转换后的对偶问题进行求解，即可得到分割超平面 。
##### 1.3.3.1 广义拉格朗日函数
&emsp;&emsp;先根据式（1-4）构造出对应的广义拉格朗日函数如下：

$$
L(\vec w, b, \vec \xi, \vec \alpha, \vec \beta) =  {\frac{1}{2} {||w||}}^2 + C\sum\_{i=1}^N \xi\_i - { \sum\_{i=1}^N \alpha\_i  [y\_i (\vec w · \vec x\_i + b) - (1 - \xi\_i)]} -  { \sum\_{i=1}^N \beta\_i \xi\_i }
\tag{1 - 5}
$$

&emsp;&emsp;式中，$\alpha\_i \geq 0, \ \beta\_i \geq 0$。另外需要补充的是，构造此广义拉个朗日函数后的附加等式条件如下：

$$
\begin{split}
\begin{cases}
& y_i \lgroup {\vec w} · \vec x_i + {b} \rgroup - (1 - \xi\_i) = 0 \\\\
\\\\
& \beta\_i \cdot\xi\_i = 0
\end{cases}
\end{split}
$$

##### 1.3.3.2 对偶问题
&emsp;&emsp;由前面章节的论述可知，原始问题的对偶问题是其广义拉格朗日函数的极大极小问题，即$\max \limits\_{\alpha} \min \limits\_{\vec w, \vec b,\vec \xi} L(\vec w, \vec b,\vec \xi, \vec \alpha, \vec \beta)$。

##### 1.3.3.3 极小值
&emsp;&emsp;先求极小值问题。将广义拉格朗日函数对$\vec w、b、\vec \xi$分别求偏导有：

$$
\begin{cases}
\begin{split}
&\nabla \_{\vec w} L(\vec w, \vec b,\vec \xi, \vec \alpha, \vec \beta) = \vec w - \sum\_{i=1}^N \alpha\_i y\_i \vec x\_i \\\\
&\nabla \_b L(\vec w, \vec b,\vec \xi, \vec \alpha, \vec \beta) = - \sum\_{i=1}^N \alpha\_i y\_i \\\\
&\nabla \_{\vec \xi} L(\vec w, \vec b,\vec \xi, \vec \alpha, \vec \beta) = \vec C - \vec \alpha - \vec \beta
\end{split}
\end{cases}
\tag{1 - 6}
$$

&emsp;&emsp;注意，上式中$\vec C = \underbrace{(C, C, \cdots, C)^T}\_{N}$，是以惩罚系数C为基本元素的N维常数向量。
&emsp;&emsp;然后分别另上式（1-6）的三个偏导数等于零，有：

$$
\begin{cases}
\begin{split}
& \vec w - \sum\_{i=1}^N \alpha\_i y\_i \vec x\_i = \vec 0\\\\
& \sum\_{i=1}^N \alpha\_i y\_i = 0\\\\
& \vec C - \vec \alpha - \vec \beta = \vec 0
\end{split}
\end{cases}
\tag{1 - 7}
$$

&emsp;&emsp;然后我们将上式（1-7）回代到式（1-5）的广义拉格朗日函数中，并对其进行化简，有：

$$
\begin{split}
& L(\vec w, \vec b,\vec \xi, \vec \alpha, \vec \beta)\\\\
&= \underbrace{ {\frac{1}{2} {||w||}}^2 - { \sum\_{i=1}^N \alpha\_i  y\_i (\vec w · \vec x\_i + b) } +  { \sum\_{i=1}^N \alpha\_i }}\_{线性可分情况下的广义拉格朗日函数} + C\sum\_{i=1}^N \xi\_i - \sum\_{i=1}^N \alpha\_i \xi\_i - \sum\_{i=1}^N \beta\_i \xi\_i \\\\
&= \underbrace{ {\frac{1}{2} {||w||}}^2 - { \sum\_{i=1}^N \alpha\_i  y\_i (\vec w · \vec x\_i + b) } +  { \sum\_{i=1}^N \alpha\_i }}\_{线性可分情况下的广义拉格朗日函数} + \sum\_{i=1}^N (C - \alpha\_i - \beta\_i) \xi\_i\\\\
&= \underbrace{ {\frac{1}{2} {||w||}}^2 - { \sum\_{i=1}^N \alpha\_i  y\_i (\vec w · \vec x\_i + b) } +  { \sum\_{i=1}^N \alpha\_i }}\_{线性可分情况下的广义拉格朗日函数} + \underbrace{(\vec C - \vec \alpha - \vec \beta)}\_{\vec 0} \cdot \vec \xi\\\\
&=  -\frac{1}{2} \sum\_{i=1}^N \sum\_{j=1}^N \alpha\_i \alpha\_j y\_i y\_j (\vec x\_i · \vec x\_j) + \sum\_{i=1}^N \alpha\_i
\end{split}
\tag{1 - 8}
$$

&emsp;&emsp;由上式可以看出，**线性非可分情况下的广义拉个朗日函数和线性可分情况下的广义拉格朗日函数的极小值问题求解结果是等价的**，即有如下结果：

$$
\underbrace{\min \limits\_{\vec w, \vec b} L(\vec w, \vec b, \vec \alpha)}\_{线性可分极小值}= \underbrace{\min \limits\_{\vec w, \vec b,\vec \xi} L(\vec w, \vec b,\vec \xi, \vec \alpha, \vec \beta)}\_{线性非可分极小值}= -\frac{1}{2} \sum\_{i=1}^N \sum\_{j=1}^N \alpha\_i \alpha\_j y\_i y\_j (\vec x\_i · \vec x\_j) + \sum\_{i=1}^N \alpha\_i
\tag{1 - 9}
$$

##### 1.3.3.4 极大值
&emsp;&emsp;求出极小值问题后，我们再来求解线性非可分情况下的极大值，即求解下列问题：

$$
\max \limits\_{\vec \alpha} [-\frac{1}{2} \sum\_{i=1}^N \sum\_{j=1}^N \alpha\_i \alpha\_j y\_i y\_j (\vec x\_i · \vec x\_j) + \sum\_{i=1}^N \alpha\_i] \\\\
\tag{1 - 10}
$$

&emsp;&emsp;当然，上式的$\vec \alpha$还必须要满足如下的约束条件：

$$
\begin{split}
\begin{cases}
\sum\_{i=1}^N \alpha\_i y\_i = 0\\\\
\\\\
\vec C - \vec \alpha - \vec \beta = \vec 0, \ C > 0 \\\\
\\\\
\alpha\_i \geq 0, \ \ \beta\_i \geq 0, \ i=1,2, \cdots, N
\end{cases}
\end{split}
\tag{1 - 11}
$$

&emsp;&emsp;因为要求对$\vec \alpha$的极大值问题，所以我们要消去上式中的$\vec \beta$，$C$是一个常数，不用理会。根据上式（1-11）的最后两个式子可求得：

$$
\begin{split}
\begin{cases}
\sum\_{i=1}^N \alpha\_i y\_i = 0\\\\
\\\\
0 \leq \alpha\_i \leq C, \ i=1,2, \cdots, N
\end{cases}
\end{split}
\tag{1 - 12}
$$

&emsp;&emsp;当然，我们如果保留$\beta\_i$而消掉$\alpha\_i$的话，可以得到关于$\beta\_i$的取值范围为：$\beta\_i \in [0, \ C]$。
&emsp;&emsp;从上式可以看出，相比于线性可分SVM（$\alpha\_i \in [0, \ \infty)$），线性非可分SVM的拉格朗日乘子$\alpha\_i$的取值区间多了一个上限约束（$\alpha\_i \in [0, \ C]$），这个上限约束正好是我们的惩罚参数。
&emsp;&emsp;从上式（1-12）可以看出，线性非可分情况下的极大值求解与线性可分情况下的极大值求解思路是一样的，仅仅是其拉格朗日乘子多了一个上限约束而已。利用式（1-12）的约束等式（第一个式子），我们可以用$N$个朗格朗日乘子中的$N-1$个去表示剩下的一个，回代到式（1-10）中，然后分别对$N-1$个拉格朗日乘子求偏导并另其等于零，从而得到$N-1$个等式。于是我们就可以解出所有的拉格朗日乘子，即我们的$\alpha\_i^\*,\  i=1,2, \cdots, N$。

##### 1.3.3.5 $\vec w^\*$求解
&emsp;&emsp;求出$\vec \alpha^\*$后，根据式（1-7）的第一个等式，可求得我们的权重系数向量，如下：

$$
\begin{split}
\vec w^\* &= \sum\_{i=1}^N \alpha\_i^\* · y\_i · \vec x\_i \\\\
&= \alpha\_1^\* y\_1 \vec x\_1 + \ \alpha\_2^\* y\_2 \vec x\_2 + \ \dots + \ \alpha\_N^\* y\_N \vec x\_N
\end{split}
\tag{1 - 13}
$$

##### 1.3.3.6 $\vec \beta\_i^\*$求解
&emsp;&emsp;求出$\vec \alpha^\*$后，我们再根据条件$\vec C - \vec \alpha - \vec \beta = \vec 0$可以求得$\beta\_i^\*$如下：

$$
\beta\_i^\* = C - \alpha\_i^\*, \ i=1,2, \cdots, N
\tag{1 - 14}
$$

##### 1.3.3.7 $\xi\_i^\*$求解
&emsp;&emsp;求出$\beta\_i^\*$后，我们再根据构造广义拉个朗日函数时的条件$\beta\_i^\* \cdot \xi\_i^\* = 0$对$\xi\_i^\*$的解讨论如下：
&emsp;&emsp;1. 当$\beta\_i^\* = 0$时，$\xi\_i^\*$可以取任意值（当然还要满足其它约束条件）。
&emsp;&emsp;2. 当$\beta\_i^\* \neq 0$时，$\xi\_i^\* \equiv 0$，这时候只能取0。
&emsp;&emsp;综上，$\xi\_i^\*$的取值如下：

$$
\xi\_i^\* =
\begin{cases}
any,  \ \ \beta\_i^\* = 0 \\\\
\\\\
0,  \ \ \beta\_i^\* \neq 0 \\\\
\end{cases}
\tag{1 - 15}
$$

##### 1.3.3.8 $b^\*$求解
&emsp;&emsp;1. 与线性可分情况下的求解类似，当$\alpha\_i^\* = 0$时，即对应的样本点不起约束作用，此时$b^\*$取任意值都能满足，即有无穷个解，此时无法利用拉格朗日乘子求解$b^\*$。
&emsp;&emsp;2. 当$\alpha\_i^\* \neq 0 且 \alpha\_i^\* = C $时，有$\beta\_i^\* = 0$，此时$\xi\_i^\*$可取任意值，此时我们仍然无法解出 $b^\*$，这样的点不受惩罚参数和松弛代价的影响。
&emsp;&emsp;**3. 当$\alpha\_i^\* \neq 0 且 \alpha\_i^\* \neq C $时（即$0 < \alpha\_i^\* < C）$，有$\beta\_i^\* \neq 0$，可知$\xi\_i^\* = 0$，此时我们再根据线性非可分情况下的KKT条件得到：$y_i \lgroup {\vec w} · \vec x_i + {b} \rgroup - (1 - \xi\_i) = 0$，利用该条件可求得 $b^\*$**：

$$
\begin{split}
b^\* &= y\_i(1 - \xi\_i^\*) -  \vec w^\* · \vec x\_i \\\\
&= y\_i(1 - \xi\_i^\*) - \sum\_{j=1}^N \alpha\_j^\* · y\_j · \vec x\_j · \vec x\_i \\\\
&= y\_i - \sum\_{j=1}^N \alpha\_j^\* · y\_j · \vec x\_j · \vec x\_i
\end{split}
\tag{1 - 16}
$$

&emsp;&emsp;由上式可知，这种情况下的偏置参数$b^\*$的求解方法与线性可分的时候相同。不同的地方在于，<font color="red">线性可分情况下求解$b^\*$时，利用的是拉格朗日系数$\alpha\_i \neq 0$的点，而线性非可分情况下的求解，其要求更严格，用来求解的点必须同时满足$\alpha\_i \neq 0 且 \alpha\_i^\* \neq C $。</font>
&emsp;&emsp;另外还需要注意的地方是，在[1.3.2小节](#singleton_prove)中我们提到了，$b^\*$的值可能并不唯一，所以最终计算出来的$b^\*$可能有多个值，**然而根据李航老师的说法，在实际应用中，我们很难遇到这种情况，一般都只会遇到上面所说的唯一解这种。**。
&emsp;&emsp;至此，线性非可分SVM的分割超平面参数$\vec w、b$均求解出来了，分割超平面的方程自然就可以得出了。

#### 1.4 支持向量
##### 1.4.1 软间隔支持向量
&emsp;&emsp;线性非可分情况下的支持向量与线性可分情况下的略有不同，其支持向量不仅仅位于间隔边界上，还会存在于间隔之中，即我们本系列[第三篇](/2017/04/30/机器学习算法系列之三：SVM3/)文章中就提到过的**缓冲区**中，这种情况下的支持向量如下示意图所示：

<img src="机器学习算法系列之三：SVM5/线性非可分情况下的支持向量.png" width="550" height="400" />
<div align='center' >图1-1　　线性非可分情况下支持向量示意图</div> 




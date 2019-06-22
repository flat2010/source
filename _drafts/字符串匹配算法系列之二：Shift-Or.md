---
title: 字符串匹配算法系列之一：Shift-Or
tags:
---

术语&&符号

- $pat$：特征串；
- $text$：负载内容(待匹配)；
- $m$：特征串$pat$的长度，$m = pat.length$；
- $n$：负载$text$的长度，$n = text.length$；
- $i$：当前正在匹配的特征串$pat$的索引(负载的第$i$个字符串)；
- $j$：当前正在匹配的负载的$text$索引(负载的第$j$个字符串)；
- $s_{i}^{j}$：将负载的第$text_j$个字符串与特征串的第$i$个字符串$pat_i$匹配后的**全局匹配状态**，它实际上也等于$pat_1, \cdots, pat_i$和$text_{j-i+1}, \cdots, text_j$两个字符串之间不相同的字符**总数**。同时，对于任意$j$，定义$s_0^j = 0$；
- $\sum$：字母表(所有可能出现的字符)；
- $|\sum|$：字母表中总字符数量(所有可能出现的字符类型数量)；
- $T$：关于特征串即字符表的预处理向量表，包含$|\sum|$个向量，每个向量包含$m$个元素，即一个$|\sum| \times m$矩阵；
- $T[x]$：字符表中字符$x$的预处理向量，维度为特征串的长度即$T[·].length = m$；
- $T_i[x] = \begin{cases} 0 \qquad \qquad x=pat_i\\ 1 \qquad \qquad 其它\end{cases}$，预处理向量第$i$个元素的取值公式，$x$为字母表字符，$pat_i$为特征串第$i$个字符；


$$
\begin{split}
text@index&: & &\downarrow^{j-i+1} & &\downarrow^j \quad & & \quad &\downarrow^{n}\\
text&: \quad \cdots \quad &a \quad c \quad \cdots \quad &a \quad a \quad b \quad c \quad &\cdots \quad c \quad &{\color{red}\fbox{b}} \quad &\cdots \quad &a \quad \cdots \quad &c\\
pat&: \quad &&a \quad a \quad b \quad c \quad &\cdots \quad c \quad &{\color{red}\fbox{a}} \quad &\cdots \quad &b\\
pat@index&: & &\uparrow_0 & &\uparrow_{i} \quad & &\uparrow_{m}\\
\end{split}
$$

- $b$：要表示每一个特征串字符在负载某个字符处匹配状态的比特位数，对于字符串匹配问题通常取1；

- $s_i$：特征串前$i$个字符在负载的某个字符处是否匹配，其计算公式为：

$$
s_{i}= \begin{cases}
0 \qquad pat_1,\cdots,pat_i = text_1,\cdots,text_j\\
1 \qquad pat_1,\cdots,pat_i \neq text_1,\cdots,text_j\\
\end{cases}
$$

&emsp;&emsp;注：Paper中$P_3$使用了$s_i^j$来表示$s_i$，这与前面$s_i^j$的定义相冲突，给读者带来不小的困惑，作者的本意是想体现匹配的位置在负载的第$j$个字符处，却没有考虑到符号的冲突。

&emsp;&emsp;一定要严格区分$s_i$和$s_i^j$的差异之处，前者体现的是整体是否匹配($s_i \in \{0, 1\}$)，后者反映不匹配的字符总数($s_i^j \in [0\ , \  i]$)。

&emsp;&emsp;为了方便表达同时不至于混淆，我们用$s_{i,j}$表示$s_i$在$j$处是否匹配！

- $w$：单个字的比特位数；

- $state_j$：读入负载第$j$个字符后的**整体匹配状态向量**；



公式解读

移动前后匹配状态的计算公式：
$$
s_i^j = s_{i-1}^{j-1} + T_i[text_j]
$$

&emsp;&emsp;这个公式有点抽象，需要点辅助图示来帮助理解。

&emsp;&emsp;假定$s_{i-1}^{j-1}=k$，即特征串的前$i-1$个字符和负载的前$j-1$个字符中有$k$个不匹配的，如下所示：
$$
\begin{array}{c:c}
\begin{split}
&&&&& \\
pat&: \quad &b \quad c \quad \cdots &a \quad &a &\\
text&: \quad \cdots \quad &a \quad c \quad \cdots \quad &b \quad &a\\
&\rightarrow &s_{i+1}^{j+1}=k &\leftarrow & & &
\end{split} & 
\begin{split}
&i&&&&\\
&b \quad c \quad &\cdots \quad c \quad &a \quad &\cdots \quad &b\\
&b \quad c \quad &\cdots \quad c \quad &b \quad &\cdots \quad &a \quad \cdots \quad &c\\
&j&&&
\end{split}
\end{array}
$$

&emsp;&emsp;进一步，若要对比特征串前$i$个字符和负载的前$j$个字符的不匹配数$s_i^j$有：
$$
\begin{array}{c:c}
\begin{split}
&&&&& \\
pat&: \quad &b \quad c \quad \cdots &a \quad &a \quad \ \ &b\\
text&: \quad \cdots \quad &a \quad c \quad \cdots \quad &b \quad &a \quad \ \ &b\\
&&&& &{\color{red}?} \\
&\rightarrow & s_i^j = k+?&&\leftarrow  &
\end{split} & 
\begin{split}
&i+1&&&&\\
&c \quad &\cdots \quad c \quad &a \quad &\cdots \quad &b\\
&c \quad &\cdots \quad c \quad &b \quad &\cdots \quad &a \quad \cdots \quad &c\\
&j+1&&& \\
&&&&
\end{split}
\end{array}
$$
&emsp;&emsp;特征串前$i$个字符和负载的前$j$个字符的不匹配数量 $=$ 特征串前$i-1$个字符和负载前$j-1$个字符不匹配数量 $+$ 移动后新增字符匹配状态。

&emsp;&emsp;更容易理解的数学表达式如下：
$$
\begin{split}
s_{i}^{j} &= \underbrace{diff(pat_1 \cdots pat_{i-1} {\color{red}pat_i}, \quad text_1 \cdots text_{i-1} {\color{red}text_j})}_{?} \\
&= \underbrace{diff(pat_1 \cdots pat_{i-1}, \quad text_1 \cdots text_{j-1})}_{k} + diff(pat_{i}, \quad text_{j}) \\
&= s_{i-1}^{j-1} + T_i[text_j] \\
&= \begin{cases}
k + 1, &\quad pat_i \neq text_j \\
k, &\quad pat_i = text_j
\end{cases}
\end{split}
$$
&emsp;&emsp;Paper中关于这部分的图示讲解（Figure1(b)）具有非常强的误导性，它本意是想表述上面的关系，但右边的$s_i^{j-1}$、$T_i[a]$、$s_i^j$三者关系的呈现方式有问题。修改后如下：
$$
(a) \qquad
\begin{array}{c|c:c}
\begin{split}
&& \\
&& \\
patten&: &&&&&& \quad &a \\
patten&: &&&&& \quad &a \quad &b \\
patten&: &&&& \quad &a \quad &b \quad &a \\
patten&: &&& \quad &a \quad &b \quad &a \quad &b\\
patten&: && \quad &a \quad &b \quad &a \quad &b \quad &c\\
\\
text&: \quad &c \quad &b \quad &b \quad &a \quad &b \quad &a \quad &{\color{blue}\fbox{b}}\\
&&&&& \\
\end{split} 
&
\begin{split}
&&\\
&&\\
&b  \quad a \quad b \quad c &\\
&a \quad b \quad c &\\
&b \quad c &\\
&c &\\
&&\\
\\
&a \quad b \quad c \quad a \quad b \quad a \quad \cdot \quad &\cdot &\\
&&& \\
\end{split}
&
\begin{split}
&i \quad  \quad &s_{i-1}^{j-1} \quad & \quad & \\
&0 &0 &&\\
&1 &1 &&\\
&2 &0 &&\\
&3 &3 &&\\
&4 &0 &&\\
&5 &5 &&\\
\\
\\
\\
\end{split}
\end{array}

\\

(b) \qquad
\begin{array}{c|c:c}
\begin{split}
&& \\
&& \\
patten&: &&&&&&& \quad &a \\
patten&: &&&&&& \quad &a \quad &b \\
patten&: &&&&& \quad &a \quad &b \quad &a \\
patten&: &&&& \quad &a \quad &b \quad &a \quad &b\\
patten&: &&& \quad &a \quad &b \quad &a \quad &b \quad &c\\
\\
text&: \quad &c \quad &b \quad &b \quad &a \quad &b \quad &a \quad &{\color{blue}\fbox{b}} \quad &{\color{red}\fbox{a}}\\
&&&&&& \\
\end{split} 
&
\begin{split}
&& \quad \\
&& \quad \\
&b  \quad a \quad b \quad c &\\
&a \quad b \quad c &\\
&b \quad c &\\
&c &\\
&&\\
\\
&b \quad c \quad a \quad b \quad a \quad \cdot \quad &\cdot \quad &\\
&&& \\
\end{split}
&
\begin{split}
&i \quad \quad &s_i^j \  = \  &T_i[a] \  + \ &s_{i-1}^{j-1} \\
&0 &— &— &—\\
&1 &0 &0 &0\\
&2 &2 &1 &1\\
&3 &0 &0 &0\\
&4 &4 &1 &3\\
&5 &0 &1 &0\\
\\
\\
\\
\end{split}
\end{array}
$$



&emsp;&emsp;如上所示，我们只需要利用上一次的匹配结果就能直接计算出这一轮的匹配结果。



全局匹配状态向量：
$$
state_j = \sum_{i = 0}^{m - 1}s_{i+1} \cdot 2^{b \cdot i}
$$
&emsp;&emsp;这个式子看起来比较绕，我们用前面自定义的符号重写这个表达式，这样更直观，如下：
$$
state_j = \sum_{i=1}^{m} s_{i,j} \cdot 2^{b \cdot (i-1)}
$$

&emsp;&emsp;即$state_j$实际上是一个由$m \times b$个比特组成的整数，每$b$个比特表示一个特征串的匹配结果，如下所示：

$$
\underbrace{\overbrace{\underbrace{\fbox{} \dots \fbox{}}_{b-bits}}^{s_{m,j}} \quad \dots  \quad\overbrace{\underbrace{\fbox{} \dots \fbox{}}_{b-bits}}^{s_{i,j}} \quad \dots \quad \overbrace{\underbrace{\fbox{} \dots \fbox{}}_{b-bits}}^{s_{0,j}}}_{state_j}
$$

&emsp;&emsp;对于字符串匹配这种应用场景来说，一般取$b = 1$，于是有：

$$
\underbrace{
\begin{split}
&\fbox{} \quad &\dots \quad &\fbox{} \ &\fbox{}\ &\fbox{} \\
&\tiny{2^{m-1}} &\cdots &\tiny{2^{2}} &\tiny{2^{1}} &\tiny{2^{0}} 
\end{split}
}_{state_j}
$$

&emsp;&emsp;由此可知，当负载中匹配到完整的特征串时，必然有：$s_{m,j} = 0$或是$state_j < 2^{m -1}$。

<details>
    &emsp;&emsp;这里有必要说明一下，若特征串$pat$在负载的$j$处完全匹配，则必然有$s_{m,j} = 0$，由$state_i$的计算公式可知：


$$
\begin{split}
state_j &= \sum_{1}^{m} s_{i,j} \cdot 2^{i-1} \\
&= s_{1,j} \cdot 2^0 + \cdots + s_{m,j} \cdot 2^{m-1} \\
&= \underbrace{s_{m,j} \cdot 2^{m-1}}_{=0} + \cdots + s_{1,j} \cdot 2^0 \\
&= \underbrace{s_{m-1,j} \cdot 2^{m-2} + \cdots + s_{1,j} \cdot 2^0}_{m-1项}\\
&\le 2^{m-2} + \cdots + 2^0 \\
& \le 2^{m-1} - 1 \\
& < 2^{m-1}
\end{split}
$$

&emsp;&emsp;即此时全局匹配状态向量必然满足$state_j < 2^{m -1}$。
</details>



特征串预处理表：
$$
T[x] = \sum_{i=0}^{m - 1} \delta(x \neq pat_{i+1}) 2^{b \cdot i}, \qquad \qquad \delta(x) = 
\begin{cases}
1, \qquad x = true \\
0, \qquad x = false \\
\end{cases}
$$

&emsp;&emsp;要存储这个表，需要$b \cdot m \cdot |\sum|$的存储空间，预处理表$T$的建立需要$O(\lceil \frac{m \cdot b}{w} \rceil (m + |\sum|))$的时间复杂度。

&emsp;&emsp;在读入一个新的负载字符后，整体匹配状态向量需要执行如下步骤更新：

- $state_i$先左移$b$个比特。这步对应的是，我们沿着负载前进了一个字符；
- 依据读入的新字符更新$s_{i,j}$。这一步通过预处理表$T$、运算符$op$、老的状态向量$state_{j-1}$求得新的状态向量$state_{j}$。注意当且仅当操作符对$s_{i,j}$的作用不会产生影响$s_{i+1, j}$的进位时才有效。

&emsp;&emsp;更新公式如下所示：
$$
state_j = (state_{j - 1} << b) \quad op \quad T[text_j]
$$
&emsp;&emsp;对于精确字符串匹配，$op$通常采用**位或**。
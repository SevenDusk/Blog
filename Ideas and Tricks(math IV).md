# 55.特征方程

以斐波那契数列数列为例

可以写出其矩阵递推的形式，其中最重要的就是求
$$
\begin{pmatrix}
0 & 1\\
1 & 1 \\
\end{pmatrix}^n
$$
显然这是若干线性变化复合得到的，由于线性变化选取的基不同，得到的矩阵也是不同的

具体来说，这两个矩阵应该满足
$$
T^{-1}AT=B \Leftrightarrow AT=TB\\
B^n=(T^{-1}AT)^n=T^{-1}A^nT\\
$$
那么由于对角矩阵的$n$次方是很好计算的，那么我们目标就是将$B$变成对角矩阵$\begin{pmatrix} \lambda_1& 0\\0&\lambda_2 \end{pmatrix}$

如果把$T$写成$(T_1\ T_2)$的形式的话

可以得到以下的线性方程组
$$
\left\{
\begin{aligned}
AT_1 & = \lambda_1I_2 T_1 \\
AT_2 & = \lambda_2I_2 T_2 
\end{aligned}
\right.
$$
移项可得$(A-\lambda_i I)T_i=0$

那么有非零解的条件根据克拉默法则就是$\det(A-\lambda_i I)=0$

对于斐波那契数列来说就是
$$
|\begin{pmatrix}
0 & 1\\
1 & 1 \\
\end{pmatrix}-\begin{pmatrix} \lambda& 0\\0&\lambda \end{pmatrix}|
=\begin{vmatrix}
-\lambda   & 1\\ 
1 & 1 -\lambda
\end{vmatrix}=\lambda^2-\lambda-1=0
$$
关于$\lambda$的多项式就是特征多项式，求出这个就可以直接得到$B$，就可以很方便计算

但是对于特征方程有重根的情况，这个方法就有问题了

比如$F_n=4F_{n-1}-4F_{n-2}$

那么有
$$
\begin{pmatrix}
F_{n} & F_{n-1} \\
\end{pmatrix}\times \begin{pmatrix}
4 & 1\\
-4 & 0 \\
\end{pmatrix}=\begin{pmatrix}
F_{n+1} & F_n\\
\end{pmatrix}
$$
那么相当于要求
$$
\begin{pmatrix}
4 & 1\\
-4 & 0 \\
\end{pmatrix}^n
$$
尝试刚才一样的方法，得到的特征方程为$(\lambda-2)^2=0$，只有唯一解$\lambda=2$，那么此时
$$
T\begin{pmatrix}
\lambda & 0\\
0 & \lambda \\
\end{pmatrix}T^{-1}=T(\lambda I)T^{-1}=\lambda TT^{-1}=\lambda I
$$
显然跟原来的矩阵$\begin{pmatrix}
4 & 1\\
-4 & 0 \\
\end{pmatrix}$不同，那么就不能构造对角矩阵

# 56.分形

一般来说分形都是具有很强的递归性，如果使用递归算法可以很清楚表示这个分形

最基本的思路应该是将图形分成若干块，然后一个高级块，是由若干低一级的块加上一些线组成的

可能根据这个思路，写出一般的递归式

对于封闭图形需要注意的是可能会有常数级别的线不满足关系，只需要按照这些线划分图形，最后将所有块和这些线拼接起来就行了

而这些图形往往是某一个图形通过旋转得到的，具有很强的对称性

还需要注意一下线段的长度问题，如果确定最低一级的单位长度，那么画出的每一条线段都是等长的

但如果需要保留比例，那么每一级的长度就需要计算，分别对长宽除以对应的线段数量

# 57.$\sum\limits_{R\subseteq S\subseteq T}(-1)^{|T\backslash S|}=[R=T]$

# 58.Ryser formula

关于一些排列计数的方法
$$
\begin{align}
\sum\limits_{f:N\rightarrow N,f(N)=N}&=\sum\limits_{R}[R=N]\sum\limits_{f(N)=R}\\
&=\sum\limits_{R}\sum\limits_{S}[R\subseteq S](-1)^{|N\backslash S|}\sum\limits_{f(N)=R}\\
&=\sum\limits_{S}(-1)^{|N\backslash S|}\sum\limits_{R}[R\subseteq S]\sum\limits_{f(N)=R}\\
&=\sum\limits_{S}(-1)^{|N\backslash S|}\sum\limits_{f:N\rightarrow S}
\end{align}
$$
对于$\sum\limits_{f:N\rightarrow S}$是表示对于所有的$f$函数求和，如果用$v(i,j),i\in N,j\in S$表示$f(i)=j$的贡献，那么
$$
\sum\limits_{f:N\rightarrow S}=\prod\limits_{i=1}^n \sum\limits_{j\in S} v(i,j)
$$
比如求二分图完美匹配的数量

## Perfect matchings in bipartite graphs

Ryser formula
$$
\sum\limits_{\pi\in S_n}\prod\limits_{i=1}^n [i\pi(i)\in E]=\sum\limits_{S\subseteq N}(-1)^{|N\backslash S|}\prod\limits_{i=1}^n\sum\limits_{j\in S}[ij\in E]
$$
证明：

其中$\prod\limits_{i=1}^n\sum\limits_{j\in S}[ij\in E]$表示的是对于所有$i$在$S$选择一个对应的方案数假设当前有函数$g:N\rightarrow N$，值域$R=g(N)$，g会贡献$1$当且仅当$R\subseteq S$

相当于$\sum\limits_{R\subseteq S\subseteq N}(-1)^{|N\backslash S|}=[g(N)=N]$

那么只有当g为一个排列的时候才能产生贡献

# 59.不定方程的解数

对于如下方程
$$
\sum\limits_{i=1}^n a_ix_i=m
$$
直接解是只能枚举的，如果$n,a_i$比较小的话，就可考虑以下的优化方法

这个是常规方法处理不了的问题，其原因常规方法绕不过枚举的问题，如果直接枚举会有很多选择空间，**说白了就是一次考虑的太多了，可能就需要将若干次的枚举组合起来**

考虑按照二进制位一位一位进行考虑，过程中需要记录进位，然后$2^n$枚举$x_i$这一维是0还是1，求出当前这一位是否跟$m$这一位相同，这个过程可以用DP进行实现

可以发现进位的最多为$n\max a$

这样的时间复杂度为$O(2^n n\max a\log m)$

# 60.对于$ax^k\bmod m$

如果$(x,m)=1$，对于最终得到的图是形成了若干个环

**对于同一个环来说其环上所有数与$m$的gcd是相同的，并且对于gcd相同的环来说，环的大小也是相同的**

证明如下，假设其$(a,m)=g$

那么$g|ax^k-pm$，那么显然$g|(ax^k\bmod m,m)$，而g是最大的因数

假设$a=gA,b=gB$其中$(A,m)=(B,m)=1$，那么$A,B$一定存在逆元

假设$gAx^k\equiv gA$那么可以推出$gx^k=g$，那么$gBx^k=gB$

# 61.FWT/高维前缀和的扩展（SOS DP）

FWT是可以处理多维并且每一维比较小并且独立的问题，假设有$k$维并且每一维的状态记作$a_i$，对于所有的状态的状态数组为$F_{a_1,a_2,...,a_k}$一般来说都是将所有维的状态压缩到一个数中（不一定要是二进制也可以是其他的进制）

那么其中某一维中，我们可以写出转移的式子，假设当前考虑的是第$i$维，如果有转移
$$
F_{a_1,...,a_{i-1},x,a_{i+1},...,a_n}\rightarrow F_{a_1,...,a_{i-1},y,a_{i+1},...,a_n}
$$
那么在FWT处理到当前这一维的时候，枚举固定前面$a_1,...,a_{i-1}$和后面$a_{i+1},...,a_n$的取值，然后将这一维的x转移到y上

当然FWT不止可以用在计数上，也可以进行最优化的转移

一个比较好的例题：https://atcoder.jp/contests/arc132/tasks/arc132_f

就是在容斥了之后，相当于确定某一些位置的取值，对于另一些位置是可以任意取值，那么每一位有4种状态，其中前三种是表示这一位只能选P,R,S中一个，对于第4个位置是表示所有的位置都能选，在预处理的时候就是将每一位的前三个状态的数值加到第4个状态上

# 62.二分图计数相关问题

首先考虑一个最简单的问题，染色二分图计数不要求连通（染色的意义是二分图的左右两部是视作不同的）

那么可以枚举左部点存在多少个点即可，记作$h_n$，然后进行化简
$$
\begin{align}
h_n&=\sum\limits_{i=0}^n \binom{n}{i} 2^{i(n-i)}\\
&=n!\sum\limits_{i=0}^n \frac{1}{i!(n-i)!}2^{\binom{n}{2}-\binom{i}{2}-\binom{n-i}{2}}\\
&=n!2^{\binom{n}{2}}\sum\limits_{i=0}^n \frac{1}{i!2^{\binom{i}{2}}}\frac{1}{(n-i)!2^{\binom{n-i}{2}}}
\end{align}
$$
这里需要特别注意一下$2^{i(n-i)}$的化简，由于目标是使用FFT进行优化，肯定要将指数上的$i,n-i$进行分离，一种化简的方式是化简成$\frac{n^2-i^2-(n-i)^2}{2}$，但是可能会出现不能被2整除的情况，就需要求出2在模数意义下的二次剩余，但是如果转化成下降幂那么一定就是可以整除的

计算的时间复杂度$O(n\log n)$

然后考虑计算二分图计数和连通二分图计数问题，答案分别记作$f_n,g_n$

其中染色二分图由若干联通块组成，并且每一个联通块可以贡献2的方案
$$
H=\sum\limits_{i} \frac{2^i G^i}{i!}=e^{2G}
$$
那么$G=\frac{\ln H}{2}$

二分图是由若干联通块组成
$$
F=\sum\limits_{i} \frac{G^i}{i!}=e^G
$$
那么可以得到$H=F^2$，那么$F=\sqrt H$（需要注意的是这里F并不是简单将$H$除以2，这是因为染色二分图不一定连通，那么同一张图被计算的次数为2的联通块数量次）

# 63.关于次幂的二项式反演优化

现在考虑一个问题，现在有$n$个特殊元素，钦定选择其中$i$个的方案数为$f_i$，恰好选择其中$i$个元素方案数为$g_i$，我们要计算$\sum\limits_{i=0}^n g_i\times  k^i$

根据二项式反演可以知道
$$
g_n=\sum\limits_{i=0}^n \binom{n}{i} (-1)^i f_i
$$
但是这个做法必须需要求出每一项$f_i$有其局限性，但是有一种更优的做法

首先在恰好$a$个方案中被钦定$b$个方案中计算了$\binom{a}{b}$次，现在考虑将$k^a$的贡献拆分
$$
k^a=(k-1+1)^a=\sum\limits_{b=0}^a \binom{a}{b}(k-1)^b
$$
那么在计算钦定方案数的时候，每加入一个元素就乘上$(k-1)$的贡献，最终$\sum\limits_{i=0}^n f_i$就是答案

# 64.LGV引理

LGV引理用于在DAG上统计$n$条不相交路径数量

首先我们需要确定一个起点集合$A$终点集合$B$，满足$|A|=|B|=n$

记$e(u,v)$表示DAG上$u$到$v$的路径总数
$$
M=\begin{bmatrix}
e(a_1,b_1)&e(a_1,b_2)&\cdots&e(a_1,b_n)\\
e(a_2,b_1)&e(a_2,b_2)&\cdots&e(a_2,b_n)\\
\vdots&\vdots&\ddots&\vdots\\
e(a_n,b_1)&e(a_n,b_2)&\cdots&e(a_n,b_n)
\end{bmatrix}
$$
那么最终$n$条不相交路径计数为$\det M$

# 65.杨表钩长公式

杨表常用于在组合学、表示理论和代数几何中，用各种不同计算杨表个数的方法得到舒尔函数的定义及相关的恒等式。在信息学竞赛中，常有考察杨表钩长公式的题目。

### 勾长

给定一个共有 $n$个方格的杨表 $\pi_{\lambda}$，把$1$到$n$的$n$个数字填入杨表中，使得每行从左到右，每列从下到上都是递增的。用$dim_{\pi_{\lambda}}$表示可以这样填的方法个数。

对于杨表中的一个方格 ，定义其 **勾长**$hook(v)$ 等于同行右边的方格数加上同列上面的方格数，再加 1（即方格本身）。

### 勾长公式

如果用$dim_{\lambda}$表示这样的方法个数，**勾长公式** 就是方法个数等于$n!$除以所有方格的勾长的乘积。
$$
dim_{\pi_{\lambda}} =\frac{n!}{\prod \limits_{x\in Y(\lambda)}hook(x)}
$$


![img](https://oi-wiki.org//math/images/young-tableau-2.png)

所以对于整数分拆$10=5+4+1$的杨表，如上图所示。有
$$
dim_{\pi_{\lambda}}=\frac{10!}{7*5*4*3*1*5*3*2*1*1}=288
$$
种方法。

# 66.分治FFT中“我卷我自己形式”

就是考虑利用分治计算
$$
f_n=\sum\limits_{i=1}^{n-1} g_i\times h_{n-i}
$$
其中$g,h$函数也是在分治过程中计算的，假设我们调用了solve(l,mid)的时候，只有$i\leq mid$位置的$g,h$算出了准确值

那么在一般过程中会使用$[l,mid]\times [0,r-l]$的形式进行卷积，但是有可能$r-l>mid$就有一部分依赖于之后的计算结果，那么就不能这样计算

可以考虑如果当前不能计算，那么就等到mid之后一部分计算出结果之后，再统计贡献

首先如果$r-l\geq l$的时候，那么此时统计$[0,mid]\times [0,mid]$的结果，将当前所有已经计算的值对右边进行贡献，缺少的贡献部分就是在第二部分补足

如果$r-l<l$的时候，此时所有的$[0,r-l]$的位置都不在当前的区间，那么现在的分治区间就是在之前分治过程中$mid$之后的那部分，那么统计$[l,mid]\times [0,r-l]$和$[0,r-l]\times [l,mid]$的贡献即可

```c++
if (r-l>=l)
{
	A=poly(h,h+mid+1)*poly(g,g+mid+1);
	for (int i=mid+1;i<=r;i++) add(f[i],A[i]);
}
else
{
	A=poly(h+l,h+mid+1)*poly(g,g+r-l+1);
	for (int i=mid+1;i<=r;i++) add(f[i],A[i-l]);
	A=poly(g+l,g+mid+1)*poly(h,h+r-l+1);
	for (int i=mid+1;i<=r;i++) add(f[i],A[i-l]);
}
```


# 20.拉格朗日反演

对于任意的函数$A,B$

使得$A=\frac{B}{f(B)}$

有$B=\sum\limits_k c_kA^k$

其中$c_k=\frac{1}{k!}\{(\frac{d}{dB})^{k-1}f(B)^k\}_{B=0}$

相当于$c_k=\frac{1}{k!}[f(B)^k]^{(k-1)}|_{B=0}$

一般来说对于形式幂级数$G$

将$z$表达成$z=\frac{G}{f(G)}$

其中$f(G)$是一个关于$G$的多项式

有$G=\sum\limits_k c_kz^k$

$c_k$带入上面的公式计算即可

这个其中某一项的值可以通过多项式快速幂，在$O(nlogn)$时间算出来

当然也可以倍增来求$O(nlog^2n)$

一般用于解一些高次的方程

# 21.生成函数的展开

对于$G=\frac{P}{Q}$形式的有理函数

将$Q$分解为$Q=\prod\limits_i (1-r_iz)^{\lambda_i}$

可以写成$\frac{1}{Q}=\sum\limits_i \sum\limits_{j=1}^{\lambda_i} \frac{A_{i,j}}{(1-r_iz)^j}$

对于$A_{i,j}$，可以两边同时乘$(1-r_iz)^{\lambda_i}$，再求$(\lambda_i-j)$次导，代入$z=\frac{1}{r_i}$

可以发现的是，右边只剩下一个$A_{i,j}(\lambda_i-j)!\times (-r_i)^{\lambda_i}$，左边带入计算即可

那么就可以解出来$A_{i,j}$

那么这个生成函数每一项的就可以简单的计算得到了

# 22.集合幂级数

主要的应用是子集卷积，其他的在$FWT$里面写过了

$c_S=\sum\limits_{i\cup j=S,i\cap j=\emptyset}a_ib_j$

注意到如果没有第二个条件，那么就是or卷积，考虑如何转化第二个条件

$i\cap j=\emptyset\Leftrightarrow |i|+|j|=|S|$

那么这就提示我们，在做$or$卷积的时候同时记录一下当前的个数

这也就引出集合占位幂级数的定义，具体的定义在Vfleaking的论文里

大概意思就是将每一个原来$[x^S]F$看作一个形式幂级数

那么对于每一位的幂级数卷起来即可

复杂度$O(n^22^n)$

一些高级运算求逆,$\ln$,$\exp$

相当于是对于$[x^S]\overline{F}$上的形式幂级数，进行相应的运算，其中$\overline{F}$表示$F$的$FWT$结果

一般都是$O(n^2)$递推求解，谁还去写巨长的$O(nlogn)$?

那么一次运算的复杂度$O(n^22^n)$

# 23.牛顿迭代解一阶微分方程

没有分治fft跑的快的东西

求解$F'=G(F)$

$F_{n+1}=\frac{1}{P}(\int P[G(F_n)-G'(F_n)F_n]dx+1)$

其中$P=e^{\int -G'(F_n)dx}$

推导不写了，这东西没用。。

# 24.交换环

对于$<M,+,*>$的运算集合，如果满足$+,*$运算有交换律，结合律，分配律，并且存在$0$和单位元的定义，那么就可以称这个集合为交换环

一些例子：形式幂级数，集合幂级数（或卷积，与卷积等）等等

记号，如形式幂级数环$F[[z]]$

那么矩阵的元可以是任意一种交换环，计算矩阵乘法或者是行列式的时候，同样运用交换环所定义的运算法则对于元进行计算

所以可以用这个东西套上矩阵之类的东西，当然也可套上矩阵树定理

# 25.矩阵树定理

一句话，构造基尔霍夫矩阵，去掉一行一列求行列式

对于无向图的生成树，对角线为每一个点的度数，其他为邻接矩阵每一个位置取相反数得到

对于有向图的内向生成树，对角线为每一个点的**出度**

对于有向图的外向生成树，对角线为每一个点的**入度**

对于有向图的生成树来说，如果其求行列式的时候去掉的第$k$行第$k$列，那么默认就是以$k$为根

其计算的答案为
$$
\sum\limits_{T}\prod\limits_{x\in T}v_x
$$

# 26.解二维递推式

列出计数$DP$后，如果是二维形式，并且数据范围给的是$O(n)$级别的话，考虑去解其递归式

一般需要再列出一个方程，与之前的方程联立，得到一阶递推关系，然后去做

# 27.Pollard Rho和Miller Robin

两个背板子的算法

首先是$Miller$ $Robin$素性测试，主要思想是利用费马小定理，随机选取若干数$a$，判断是否都有$a^{p-1}\equiv 1(\bmod p)$成立，但是存在一类合数，使得这个条件成立，那么就会有概率出错

> 二次探测定理
>
> $x^2\equiv 1(\bmod p)$，$p$为奇质数，其解仅为$x\equiv 1$或$x\equiv p-1$

那么基于这个定理，可以改进上面的做法，首先将$p-1$分解成$d2^r$形式，先计算$a^d$，看其是否等于$1$，如果等于$1$之间判断条件为真，否则不断平方$r$次，如果中间出现$p-1$那么条件为真，否则为假

那么只要选取合适的a，一般选取$2,3,5,7,11,13,17,47$，$8$个质数判断即可

$Pollard$ $Rho$也是一个随机化算法，这是基于生日悖论，采用组合抽样法，来保证时间复杂度为$O(n^{\frac{1}{4}})$

首先，如果我们直接像试除法去随机求因子，那么显然不行，那么考虑用gcd加速这个过程，因为只要$gcd$不为$1$，那么$gcd$一定是$n$的因数，那么就变成了一个子问题，递归下去求解，并且随机到$gcd$不为$1$的概率很高

注意这里选取的伪随机函数为$f(x)=x^2+c$，$c$为随机的一个常数，这个函数不断迭代分布还是比较均匀

倍增优化

注意到如果每一次都求一次$gcd$效率太低，$gcd$有些比较好的性质

$gcd(a,b)=gcd(ac\% b,b)$

那么我们每次固定一个上次倍增结束的数，然后不断累乘，只到某一个时间，与$n$求$gcd$，如果$>1$就直接返回

实践证明每$128$次求一次$gcd$，效果较好

需要注意的是，如果此处$n$为质数，那么会一直死循环下去，那么这时候就需要$Miller$ $Robin$来快速判断是否为质数

# 28.单位根反演

$[k|n]=\frac{1}{n}\sum\limits_{i=0}^{k-1}\omega_{k}^{in}$

如果$k|n$，那么$\omega_{k}^{in}=1$

否则等比数列求和

# 30.原根的一些性质

设原根为$g$，那么在模$p$意义下，对于$i\in[0,\varphi (p))$，$g^i$两两不同

那么对于某一个不是原根的数，假设可以写成$g^k$，其周期是$\frac{\varphi(p)}{gcd(k,\varphi(p))}$

那么检验原根的方法就是对于$\varphi(p)$每一个质因子$x$，检查$g^{\frac{\varphi (p)}{x}}$是否为$1$，如果全部条件都不满足，那么说明当前的$g$是原根

对于$p$为质数的情况，一定存在原根，并且在$[0,p)$范围内的数一定可以通过$g$的不同次方表示出来，求阶这个可以通过$BSGS$求出，如果对于多次询问，可以调整块长，保证平衡复杂度

upd:$BSGS$是朴素实现方式（不过$BSGS$可以求出$k$具体是多少，但下面这个方法只能求出周期）

注意到周期为$\frac{\varphi(p)}{gcd(k,\varphi(p))}$，考虑确定$gcd(k,\varphi(p))$，用$\varphi(p)$的质因子去试除$\varphi(p)$，找到最大的次数，使得得到的结果是$1$，那么这个除数就一定是$gcd(k,\varphi(p))$的一个因子

复杂度$O(log^2p)$

如果$p$不为质数，可能存在原根，如果存在原根，那么那些与$p$互质的数可以用原根表示出来

对于**奇质数，奇质数的幂次，奇质数幂次的两倍，$2,4$**是存在原根的，其他整数都不存在原根

对于奇质数的幂次$p^k$，设$g$为$p$的原根，那么$p^k$的原根为$g$或者$g+p$，对于$2p^k$，$g,g+p,g+p+p^k,g+p^k$为其原根

# 31.Prufer序列

先记两个结论，对于$n$阶完全图，其生成树的方案数为$n^{n-2}$

对于$n$个节点的图，其中有$k$个联通块，每一联通块的大小为$s_i$，连成一颗树的方案数为$n^{k-2}\prod s_i$

这都是要有标号的情况下

Prufer序列就是每一次将编号最小的叶子删除然后在序列中加入这个叶子节点连接的节点，那么每一个点在序列中出现次数就是度数减1次

那么对于固定一个度数序列$d$

那么有
$$
\binom{n-2}{d_1-1,d_2-1,...,d_n-1}
$$
的方案数

# 32.平行求和法

容易忘记
$$
\sum\limits_{k\leq n}\binom{r+k}{k}=\binom{n+r+1}{n}
\\ \sum\limits_{k\leq n}k\binom{r+k}{k}=(r+1)\binom{n+r+1}{n-1}
\\ \sum\limits_{k\leq n}k^2\binom{r+k}{k}=(r+1)\binom{n+r+1}{n-1}+(r+1)(r+2)\binom{n+r+1}{n-2}
$$


可以通过画杨辉三角直观的看出

# 33.有标号计数中将标号分配到集合中的不重计数

比如当前问题的规模是$n$（标号都是从$1$到$n$），现在要加入一个大小为$m$的集合，给其分配标号

那么考虑$n+m$号元素强制是在这个集合中，那么剩下$m-1$个元素要在$n+m-1$中选择

方案就是$\binom{n+m-1}{m-1}$

# 34.$\sum\limits_{i=1}^{n} a_i\leq R$求方案数

可以像线性规划一样加入一个新的变量$B$

令$B+\sum\limits_{i=1}^{n} a_i= R$

然后直接插板法$\binom{R+n}{n}$

如果正常做就是平行求和法

# 35.等比矩阵

构造元素为矩阵$A$的如下矩阵
$$
\begin{bmatrix}
 A& A\\ 
 0& 1
\end{bmatrix}^k
=\begin{bmatrix}
 A^k& \sum\limits_{i=0}^k A^i\\ 
 0& 1
\end{bmatrix}
$$

# 36.转置原理

若$A=E_1E_2E_3...E_k$那么$A^T=E^T_kE^T_{k-1}...E^T_1$

说人话

![捕获](D:\Blog\image\捕获.PNG)

通过这个可以得到多点求值的新做法

给定多项式$F(x)=\sum_{i=0}^{n-1}f_ix^i$

$ans_i=F(q_i),i\in[0,m)$
$$
\begin{bmatrix}
f_0&f_1&\cdots&f_{n-1}
\end{bmatrix}
\times 
\begin{bmatrix}
q_0^0&q_1^0&\cdots&q_{m-1}^0\\
q_0^1&q_1^1&\cdots&q_{m-1}^1\\
\vdots&\vdots&\ddots&\vdots\\
q_0^{n-1}&q_1^{n-1}&\cdots&q_{m-1}^{n-1}\\
\end{bmatrix}
=
\begin{bmatrix}
ans_0&ans_1&\cdots&ans_{m-1}
\end{bmatrix}
$$
转置就是
$$
\begin{bmatrix}
g_0&g_1&\cdots&g_{m-1}
\end{bmatrix}
\times 
\begin{bmatrix}
q_0^0&q_0^1&\cdots&q_0^{n-1}\\
q_1^0&q_1^1&\cdots&q_1^{n-1}\\
\vdots&\vdots&\ddots&\vdots\\
q_{m-1}^0&q_{m-1}^1&\cdots&q_{m-1}^{n-1}\\
\end{bmatrix}
=
\begin{bmatrix}
b_0&b_1&\cdots&b_{n-1}
\end{bmatrix}
$$
详见[转置原理口胡 - 菜狗xzz - 博客园 (cnblogs.com)](https://www.cnblogs.com/xzz_233/p/13607680.html)

# 37.再论斯特林数

抛出两个等式
$$
(e^x-1)^m=m!\sum\limits_{n\geq 0}\left\{\begin{matrix}n\\m\end{matrix}\right\}\frac{x^n}{n!}
\\
(\ln \frac{1}{1-x})^m=m!\sum\limits_{n\geq 0}\left[\begin{matrix}n\\m\end{matrix}\right]\frac{x^n}{n!}
$$
第一个证明比较好证，直接二项式展开，得到的就是第二类斯特拉数的通项形式

第二个证明可以利用组合意义
$$
f(x)=\ln\frac{1}{1-x}=\sum\limits_{i\geq 1} \frac{x^i}{i}=\sum\limits_{i\geq 1}\frac{(i-1)!x^i}{i!}
$$
考虑$[x^n]f(x)^m$的组合意义，相当于是将$n$划分成$m$段，每一段的贡献，如果设长度为$len$,为$(len-1)!$

那么相当于是这个段做圆排列的方案数，那么总方案数就是将$n$的排列划分成$m$个置换环的方案数

就是$\left[\begin{matrix}n\\m\end{matrix}\right]$

# 38.高斯整数

形如$a+bi,a,b\in Z$的形式的复数就称之为高斯整数，高斯整数形成了一个交换环,$1,-1,i,-i$都是其单位元

在高斯整数意义下唯一分解定理仍然成立

那么不能再分解的高斯整数就被称之为高斯素数

费马二平方定理指出对于实数域上的奇素数$P\equiv 1(\bmod 4)$那么$P$可以被分解成$(a+bi)(a-bi)$的形式，

四平方定理指出任意一个整数都可以被拆分成$4$个数的平方和

# 39.下降幂多项式

定义$F(x)=\sum\limits_{i}f_ix^{\underline{i}}$

在下降幂多项式中往往会用到点值的$EGF$，主要是因为$\frac{i^{\underline{n}}}{i!}=\frac{1}{(i-n)!}$

首先考虑$F(x)=x^{\underline{n}}$点值的$EGF$
$$
\begin{equation}
G(x)=\sum\limits_{i}\frac{i^{\underline{n}}}{i!}x^i=\sum\limits_{i}\frac{x^i}{(i-n)!} =x^ne^x
\end{equation}
$$
考虑一般情况
$$
\begin{align}
G(x)&=\sum\limits_{i}(\sum\limits_{j}a_ji^{\underline{j}})\frac{x^i}{i!}\\
	&=\sum\limits_{j}a_j\sum\limits_{i}\frac{i^{\underline{j}}}{i!}x^i\\
	&=\sum\limits_{j}a_jx^je^x\\
	&=e^x\sum\limits_{i}a_ix^i
\end{align}
$$
那么只要将系数看作普通多项式再卷上$e^x$，那么就得到了下降幂多项式的点值

如果反过来已知下降幂多项式的点值，那么只要卷上$e^{-x}$即可得到系数
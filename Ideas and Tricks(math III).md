# 40.$n^2$求逆，ln，exp

集合幂级数中，或者一些非模ntt模数的做法

## 求逆

$$
\frac{1}{F}=G
\\FG=1
$$


$$
\\\sum\limits_{k=0}^ig_kf_{i-k}=0
\\g_if_0+\sum\limits_{k=0}^{i-1}g_kf_{i-k}=0
\\ g_i=\left\{
\begin{aligned}
&\frac{1}{f_0}&(i=0) \\
 &-\frac{1}{f_0}\sum\limits_{k=0}^{i-1}g_kf_{i-k} &(i\neq0)\\
\end{aligned}
\right.
$$


## ln

$$
\ln F=G
\\\frac{F'}{F}=G'
\\F'=F*G'
$$


$$
\begin{align}
\\f'_i&=\sum\limits_{k=0}^{i}g'_kf_{i-k}
\\(i+1)f_{i+1}&=(i+1)g_{i+1}f_0+\sum\limits_{k=0}^{i-1}(k+1)g_{k+1}f_{i-k}
\\g_{i+1}&=f_{i+1}-\frac{1}{i+1}\sum\limits_{k=0}^{i-1}(k+1)g_{k+1}f_{i-k}
\end{align}
$$

## Exp

$$
e^F=G
\\F'e^F=G'
\\F'G=G'
$$

$$
\begin{align}
g'_i&=\sum\limits_{k=0}^ig_{i-k}f'_{k}
\\g_{i+1}&=\frac{1}{i+1}\sum\limits_{k=0}^{i}(k+1)f_{k+1}g_{i-k}
\end{align}
$$

## 多项式$n$次方

$$
G=F^n\\
G'=nF^{n-1}F'\\
G'F=nGF'
$$

然后将这个式子展开成某一项的卷积，那么就可以通过递推求出某一项的结果

如果要求前$k$项的值，那么复杂度就是$O(k^2)$

# 41.欧拉变换

无标号计数，对应着有标号计数的exp

这里将无标号个体分配到无标号集合中

计算的是$\prod\frac{1}{(1-x^i)^{f_i}}$
$$
\begin{align}
F&=\prod \frac{1}{(1-x^i)^{f_i}}
\\\ln F&=\sum \ln \frac{1}{(1-x^i)^{f_i}}
\\\ln F&=-\sum f_i\ln (1-x^i)
\\(\ln F)'&=\sum f_i\frac{ix^{i-1}}{1-x^i}
\\ \frac{F'}{F}&=\sum f_iix^{i-1}\sum\limits_{j} x^{ij}
\\ \frac{F'}{F}&=\sum f_i\sum\limits_{j\geq 1} x^{ij-1}i
\\ \int \frac{F'}{F}&=\sum f_i\sum\limits_{j\geq 1} \frac{x^{ij}}{j}
\\\ln F&=\sum\limits_{j\geq 1} \frac{f(x^j)}{j}
\end{align}
$$
那么只要$O(n\ln n)$调和级数预处理，然后exp

# 42.Dilworth定理

在偏序集中，最小链覆盖等于最长反链

链 : $D$中的一个子集$C$满足$C$是全序集及$C$中所有元素都可以比较大小

反链 : $D$中的一个子集$B$满足$B$中任意非空子集都不是全序集 即所有元素之间都不可以比较大小

链覆盖 : 若干个链的并集为D ，且两两之间交集为∅

反链覆盖 : 若干个反链的并集为$D$ ，且两两之间交集为∅

最长链 : 所有链中元素个数最多的 (可以有多个最长链)

最长反链 : 所有反链中元素个数最多的 (可以有多个最长反链

偏序集高度 : 最长链的元素个数
偏序集宽度 : 最长反链中的元素个数

最小链覆盖（使链最少）= 最长反链长度 = 偏序集宽度
最小反链覆盖=最长链长度=偏序集深度

在偏序集形成DAG中，任意两个在链中的元素，一定可以从某一个点到达另外一个点

任意两个在反链中的元素，互相都不能到达

一种直观的理解是，如果偏序集中最多有若干的个元素两两直接不能到达（比较大小），那么至少需要这么的链去覆盖这些元素，那么最小链覆盖就是等于最大反链

# 44.Sperner定理

有一个$n$元集合$S_n$，从中选出若干个子集，满足没有任何两个子集之间存在包含关系，最多可以选出$\binom{n}{\lfloor \frac{n}{2} \rfloor}$个子集

直观理解就是从中选出$k$元集合，这样集合之间两两不互相包含

扩展到可重集就是，每一维大小为$p_i$，从中可以选出$[1,p_i]$个数

定义偏序关系为，对于$S=(s_1,s_2,s_3,...,s_n),T=(t_1,t_2,t_3,...,t_n),S\leq T$，对于任意的$i$，都满足$s_i\leq t_i$

答案就是满足$\sum x_i=\lfloor\frac{1}{2}\sum (p_i+1) \rfloor$的$x$的数量

不重集的[证明](https://www.cnblogs.com/suncongbo/p/10321099.html)

[102114F - Fireflies](https://codeforces.com/gym/102114/problem/F)

# 45.五边形数定理

定义欧拉函数为$\phi(x)=\prod\limits_i(1-x^i)$，五边形数通项$\frac{n(3n-1)}{2}$
$$
\phi(x)=\prod\limits_i(1-x^i)=1+\sum\limits_{i\geq 1} (-1)^ix^{\frac{i(3i\pm 1)}{2}}
$$
那么拆分数就可以通过多项式求逆$O(n\log n)$

# 46.关于组合数

对于一些问题，如果出现了组合数$\binom{n+m}{n}$，需要考虑转化为网格图上从$(0,0)$走到$(n,m)$的模型

# 47.$n$个点$m$条边的联通图个数

首先如果没有$m$的限制，那么可以用多项式直接做到$O(n^2)/O(n\log n)$

现在考虑$m$的限制，设$dp(n,m)$表示用$n$个点$m$条边构成的联通块数量，转移的时候考虑容斥，把所有情况数量减去图不连通的情况，枚举$1$所在联通块的点数和边数
$$
dp(n,m)=\binom{\binom{n}{2}}{m}-\sum\limits_{a=1}^{n-1}\sum\limits_b dp(a,b)\binom{n-1}{a-1}\binom{\binom{n-a}{2}}{m-b}
$$
直接转移是$O(n^6)$的，可以利用斯特林反演或者拉格朗日插值做到$O(n^4)$

第三种做法，考虑写出$F_n$的生成函数
$$
F_n=(x+1)^{\binom{n}{2}}-\sum\limits_{i=1}^{n-1} \binom{n-1}{i-1}F_i(x+1)^{\binom{n-a}{2}}
$$
然后写成$F_n=\sum\limits_i f_i(x+1)^i$的形式，其中$F_1=(x+1)^0$，然后多项式平移，最后转回正常形式，复杂度$O(n^4)$

# 48.对于步数可能走到无限的期望问题

由于这个步数可能到达$\infin$，对于这种问题算期望的⼀种方法是：对于所有还没有停⽌的情况，算它们的概率的和。

在$DP$的时候，可以考虑先将步数放入状态中去，然后列出方程，再考虑将这一维去掉，可以通过对所有步数求和，得到一个总的方程，然后再用一个新的$DP$状态替换

比如$f(i,x)$表示走了$i$步，其他状态为$x$的概率，令$g(x)=\sum\limits_i f(i,x)$，利用$f(i,x)$列出的方程，列出$g(x)$的方程

# 49.求期望的技巧

$$
\begin{align}
E&=\sum\limits_{x} x*Pr[X=x]\\
&=\sum\limits_{x} Pr[X\geq x]
\end{align}
$$

将**恰好**转化为**至少**，可以更方便的求出答案

# 50.环排列

如果所求排列是环排列，那就钦定一种选法然后乘上环的大小即可

比如钦定一个加入的点是放在第一个位置上的

# 51.矩阵乘法

$n*m$的矩阵可以根$m*p$的矩阵相乘得到$n*p$的矩阵

行列式的性质
$$
|A||B|=|AB|
$$

# 52.多项式的展开

如果$f(x)$只有有限项，并且$f(x)=\frac{p(x)}{q(x)}$，那么可以直接列竖式，进行多项式除法这样一定可以除尽，得到$f(x)$

# 53.线性代数

## 线性变换 linear transformation

定义$T:R^n\rightarrow R^m$​满足，其中$R^n$称为domain，$R^m$称为codomain，$\{T(x)|x \in R^n\}$​称为range
$$
T(u+v)=T(u)+T(v)\\
T(cu)=cT(u)\\
$$
可以得到一个推论$T(0)=0$​

定义一个transformation $T:R^n\rightarrow R^m$是one-to-one为，对于每一个向量$b\in R^m$，等式$T(x)=b$在$R^n$中**最多**有一个解

定义一个transformation $T:R^n\rightarrow R^m$是one-to-one为，对于每一个向量$b\in R^m$，等式$T(x)=b$在$R^n$中**至少**有一个解

## 线性变换求矩阵（ linear transformation and  Matrix transformations）

### 标准单位向量 The Standard Coordinate Vectors

$$
e_1=\begin{pmatrix}
1\\ 
0\\ 
...\\ 
0\\ 
\end{pmatrix}
,
e_2=\begin{pmatrix}
0\\ 
1\\ 
...\\ 
0\\ 
\end{pmatrix}
,
e_n=\begin{pmatrix}
0\\ 
0\\ 
...\\ 
1\\ 
\end{pmatrix}
$$

The $i$-th entry of $e_i$ is equal to 1, and the other entries are zero

根据上面的性质可以得到$T(x)=Ax$，其中$A$称为$T$的standard matrices
$$
A=\begin{pmatrix}
| & | & ... & |\\
T(e_1) & T(e_2) & ... & T(e_n) \\
| & | & ... & |\\
\end{pmatrix}
$$

## 线性变换的嵌套  composition

$$
T(U(x))=T\circ U(x)
$$

左结合的形式，并且**不具有交换律**

## 矩阵乘法

$$
B=\begin{pmatrix}
| & | & | & ... & |\\
v_1 & v_2 & v_3 & ... & v_p\\
| & | & | & ... & |
\end{pmatrix}\\
AB=\begin{pmatrix}
| & | & | & ... & |\\
Av_1 & Av_2 & Av_3 & ... & Av_p\\
| & | & | & ... & |
\end{pmatrix}\\
$$

-  In order for $AB$ to be defined, the number of rows of $B$ has to equal the number of columns of A.

- The product of an $m × n$ matrix and an $n × p $ matrix is an $m × p$ matrix.

## Products and compositions

如果设$T:R^n \rightarrow R^m,U:R^p \rightarrow R^n$，并且他们的standard matrices分别记作$A,B$

那么$T\circ U(x)=(AB)x$

## 逆矩阵 Matrix Inverses

### Invertible Matrices

We say that $A$ is invertible if there is an $n × n$ matrix $B$ such that
$$
AB=I_n\ and\ BA=I_n
$$
一些性质
$$
(A^{-1})^{-1}=A\\
(AB)^{-1}=B^{-1}A^{-1}
$$

## 构造逆矩阵的方法

$$
(A\ |\ I_n)\rightarrow (I_n\ |\ B)
$$

将$A$通过行变换变成$I_n$，同样施加在$I_n$上，如果无法达成，那么$A$ non-invertible

事实上这就是在解一个线性方程组
$$
Ax_1=e_1,Ax_2=e_2,...,Ax_n=e_n
$$
**关键是每一行是不是都存在主元**

# 54.抽象代数 Groups and Subgroups

一个集合元素的个数称为Cardinality
$$
\begin{align}
z&=a+bi\\
&=|z|(\cos \theta+i\sin\theta)
\end{align}
$$
Euler’s Formula
$$
e^{i\theta}=\cos \theta+i\sin\theta
$$
代入上式可以得到
$$
z=|z|e^{i\theta}
$$
由此可以推导出复数相乘的法则，模长相乘，辅角相加，由此可以得到解形如$z^4=-16$的方法
$$
|z|^4(\cos 4\theta+i\sin 4\theta)=16(-1+0i)
\Leftrightarrow |z|=2,\cos 4\theta=-1,\sin 4\theta=0
$$
对三角函数进行求解就可以了

定义$\mathbb{R}_{2\pi }$为$[0,2\pi)$上的实数，并且加法为modulo 2π下进行，用$+_{2\pi}$表示这种加法

定义$U=\{z\in \mathbb{C} ||z|=1\}$​（这个集合对于乘法和加法来说是封闭的）

有以下关系
$$
{if}\ z_1\leftrightarrow \theta_1\ and\ z_2\leftrightarrow \theta_2\ then\ z_1z_2\leftrightarrow (\theta_1 +_{2\pi} \theta_2)
$$
满足one-to-one correspondence

$\mathbb{R}_{2\pi }\ with\ +_{2\pi}$​和$U\ with\ *$​具有相同的代数结构，这种one-to-one correspondence称为同构isomorphism 

**只要满足上面的这种关系，就可以称两个代数结构同构**

原根（$n^{th}$​roots of unity）的定义$\cos(m\frac{2\pi}{n})+i\sin(m\frac{2\pi}{n}),m=0,1,2,...,n-1$

记$U_n$为所有n次原根的集合，那么$U_n\ with\ *$和$\mathbb{Z}_n\ with\ +_n$​是同构的

## Binary operation

定义：A binary operation $∗$ on a set $S$ is a function mapping $S × S$ into $S$

如果H是S的子集，并且H在原来S的Binary operation的一部分下是封闭的，称$\cross$为H的induced operation

满足交换律称为commutative，结合律称为associative

复合一定有结合律即$f\circ (g\ \circ h)=(f\circ g)\circ h$



如果一个*不是对于所有的二元组$(a,b)\ a,b\in s$都有定义，那么称这个operation是not everywhere defined

如果对于一个二元组$(a,b) a,b\in s$*操作存在多个值，那么称这个operation是not well defined

如果这个操作不封闭，那么称这个operation是not closed under ∗

只有满足以下两个条件的operation称为Binary operation

1. exactly one element is assigned to each possible ordered pair of elements of S,
2. for each ordered pair of elements of S, the element assigned to it is again in S.

## Isomorphic binary structure

给出两个代数结构同构的严谨定义

对于两个代数结构$<S,*>,<S',*'>$，同构isomorphism定义为一个将$S$到$S'$的函数$\phi$满足one-to-one和onto使得
$$
\phi(x*y)=\phi(x)*'\phi(y),for\ all\ x,y\in S
$$
称$S,S'$为isomorphic binary structures，写作$S\simeq S'$

单位元identity element

定理：如果$<S,*>$对于$*$存在单位元$e$，设$\phi:S\rightarrow S'$是$<S,*>,<S',*'>$的isomorphism，**那么$\phi(e)$就是$S'$​​的单位元**

## 群Group

一个群$<G,*>$是一个集合$G$，在Binary operation *下是封闭的，满足以下三个公理axioms

- 结合律，$a*(b*c)=(a*b)*c$
- 存在单位元$e$，$e*x=x*e=x$
- 对于每一个元素存在逆元，$a*a'=e$

一个群如果是阿贝尔群，需要其binary operation满足交换律

A group $G$​ is **abelian** if its binary operation is commutative.

群$G$的阶order定义为$|G|$，就是群中集合的大小​

### 定理1

如果$G$是一个with binary operation *的群，left and right cancellation laws hold in G
$$
a*b=a*c\Leftrightarrow b=c\\
b*a=c*a\Leftrightarrow b=c
$$
证明只需要同乘一个a的逆元即可

### 定理2

对于$a*x=b$和$y*a=b$的等式来说，存在唯一的解$x,y\in G$

证明可以先构造一组解，然后利用反证法证明这个解唯一

### 定理3

一个群中存在唯一的单位元，对于每一个元素来说存在唯一的逆元

证明也是利用反证法

### 推论

对于$a,b\in G$，我们有$(a*b)'=b'*a'$（非常像矩阵逆元的性质，事实上对于所有$n*n$矩阵和矩阵乘法构成代数结构就是一个群）



对于一些二元代数结构来说可能不满足群的一些公理，对于这些代数结构也有定义

半群semigroup：a set with an associative binary operation，binary operation具有结合律

幺半群monoid：A monoid is a **semigroup** that has an identity element for the binary operation，注意是在							半群的的基础上进行定义的

事实上如果一旦存在左单位元或者左逆元的话，那么一定存在两个方向上的单位元和逆元

## 子群

定义：If a subset $H$ of a group $G$ is closed under the binary operation of $G$ and if H with the induced operation from $G$​ is itself a group，记作$H\leq G$，如果$H\neq G$，记作$H< G$

也就是不仅是集合要是原来集合的子集，定义的运算也需要是原来导出的

判定定理

1. $H$ is closed under the binary operation of $G$,
2. the identity element $e$ of $G$ is in $H$,
3. for all $a\in H$  it is true that $a^{-1}\in H$ also.

### 循环群 Cyclic Subgroups

定理：对于群G，令$a\in G$，$H=\{a^n|n\in \mathbb{Z}\}$.**$H$是$G$的一个子群，并且$H$是所有$G$的子群中包含$a$最小的那个**

记作$<a>=H$，$a$生成generate $H$，称$H$cyclic

#### 性质一

对于任意一个循环群$G=<a>$，其都是阿贝尔群

证明:设$g_1,g_2\in G,g_1=a^r,g_2=a^s$
$$
g_1g_2=a^ra^s=a^{r+s}=a^sa^r=g_2g_1
$$
满足交换律，所以是阿贝尔群

#### 性质二

任何一个循环群的子群仍然是循环群

（就是一个环，从一个点出发以步长为k出发，走出来的路径仍然是一个环）

#### 性质三

对于任何一个循环群$G$来说

- 如果$G$的阶是无限的 ，那么$G$与$<\mathbb{Z},+>$都是同构的
- 如果$G$的阶为$n$，那么$G$与$<\mathbb{Z}_n,+_n>$是同构的

证明就是按照同构的证明方式四步走，其中第一个中的$G$，对于任意的$m\neq 0,a^m\neq e$，那么只需要构造$\phi(a^m)=m$就可以了，第二个的n相当于是周期，就是第一个$a^n=e$的位置，证明也是一样的

#### 性质四

对性质二的具体说明，如果$G=<a>,|G|=n$，对于$b=a^s$来说$|<b>|=\frac{n}{(n,s)}$

并且如果$(n,s)=(n,t)$，那么$<a^s>=<a^t>$

就是改变步长的



对于上面讨论的循环群来说，就是讨论最小的$G$子群包含$a$这个元素的子群，接下来讨论的就是包含多个元素的最小子群

任意多个子群的交集，仍然是原来群的子群

对于一个最小的子群$H\leq G$，包含$\{a_i|i\in I\}$是subgroup generated by$\{a_i|i\in I\}$，如果生成了整个$G$，那么称$a_i$是$G$的generator，如果存在有限个集合能生成$G$，那么称G is finitely generated

同时生成的集合也可以用画图来表示，对于所有属于G的元素，在图上都表示一个点，然后每一个点通过生成集合中的元素进行一次运算得到的结果就是其指向的点




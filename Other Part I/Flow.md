一些定义

$\partial^+(v)$表示所有$v$​节点的出边形成的集合

$\partial^-(v)$​表示所有$v$​​节点的入边形成的集合

$x$记作源点，$y$记作汇点，$I$​为所有不是源点和汇点的点集，$A$为边集

# 流 flow

定义一个在边集的函数$f:A\rightarrow \mathbb{R}$

一个流需要满足$f^+(v)=f^-(v),v\in I$

并且$0\leq f(a)\leq c(a),a\in A$

我们记$val(x)=f^+(x)-f^-(x)$，相当于就是流出这个点/集合的流量

对于所有除了源点和汇点的顶点来说$val(x)=0$

## 定理

对于一个流$f$​，对于任意一个子集$X$​，满足$x\in X,y\in V\backslash X$​

那么$val(f)=f^+(X)-f^-(X)$

证明:$f^+(X)-f^-(X)=\sum\limits_{i\in X}f^+(i)-f^-(i)=f^+(x)-f^-(x)=val(f)$​

最大流就是令$val(f)$最大

# 割 cut

一组割记作$k$，$cap(k)$​表示割中集合边容量之和

左侧点集记作$X$，$k=\partial^+(X)$

## 定理

对于所有的流$f$​，和割$k$​，满足$val(f)\leq cap(k)$​

当且仅当$\partial^+(X)$​中的边都满流了，$\partial^-(X)$​中都是0流量才能取等

证明：根据上面的定理可以得到$val(f)=f^+(X)-f^-(X)$​

其中$f^+(X)=\sum\limits_{i\in X}f^+(i)$，由于$f(a)\leq c(a)$，那么$f^+(i)\leq cap(\partial^+(i))$

那么得证

由于可以取等，那么显然最大流等于最小割

# The Max-Flow Min-Cut Theorem

假设$P$是一条从x出发的路径

定义$\epsilon(P)=\min\{\epsilon(a):a\in A(P)\}$

其中
$$
\epsilon(a)=\left\{
\begin{aligned}
c(a)-f(a)&\  &forward\\
f(a)&\ &reverse
\end{aligned}
\right.
$$
就是在正常边和反向边，正常边和反向边加起来就是$c(a)$

如果我们找到一条路径$P$，满足$\epsilon(P)>0$，那么就找到了一条增广路径可以令
$$
f'(a)=\left\{
\begin{aligned}
f(a)-\epsilon(P)&\  &forward\\
f(a)+\epsilon(P)&\ &reverse\\
f(a)&\ & otherwise
\end{aligned}
\right.
$$
$f'$是一个新的流

## 定理 1

如果某一个流，存在增广路径，那么这个流一定不是最大流

证明：可以构造如上的流，由于这是一条从x出发的路径，那么$f^+(x)$增加了$\epsilon(P)$，那么$val(f')=val(f)+\epsilon(P)$

## 定理 2

当某一个残量网络上不存在增广路径的时候，当前的流就是最大流，并且从原点出发经过没有满流的边可以到达的节点记作$X$，那么最小割就是$\partial^+(X)$

证明：由于不存在增广路，那么$X$​和$V\backslash X$之间不存在还可以通过的边，否则就会存在增广路

通过以上两个定理就可以得出，最大流等于最小割的结论



至于寻找增广路的方法就是bfs，强制只能通过还没有满流的边，如果$x,y$还联通，那么就说明存在增广路经，然后得出的这个搜索树叫做IPS-tree

这个算法保证每一次都会对流val(f)增加，由于最大流一定有上限，那么这个算法一定会在有限的时间内结束

#  Arc-Disjoint Directed Paths

问题就是从x到y，最多可以找出多少条不相交的路径（起点和终点不算）

首先定义循环流$f:A\rightarrow \mathbb{R}$，满足$f^+(x)=f^-(x),x\in V$

假设这个图有$n$个点，$m$条边，定义$M$为$n*m$矩阵，第i列表示第i条边，起点的行为1，终点的行为-1

那么显然有$Mf=0$

定义一个函数的support 为一个集合$A$，满足$\forall x\in A,f(x)\geq 0$​

## 定理 1

如果存在一个非零循环流$f$，那么$f$的support 一定包含一个环

证明：由于一个环的f一定为0，满足support的条件

## 定理 2

一个非零循环流$f$一定是有若干有关环的循环流线性组合得到的

证明：对于$f$​的support来说，一定存在一个环C（根据定理1可以知道），由于$f(c)=0$​，那么将这个support去掉这环，仍然是$f$的support，那么依次下去，就可以证明了

## Menger’s Theorem

最多从x到y的不相交路径等于一个源点为x，汇点为y，并且所有边的容量为1的最小割的大小

证明：假设最小割将图分成了$X$​和$Y=V\backslash X$​两个集合，并且满足$x\in X$​和$y\in Y$​

首先需要考虑最大流增广过程，由于加入的反向边，那么在增广一次路径的时候，虽然可能有些边被反悔了，但是总体上历史选择的路径数量+1

就是考虑之前的一条路径$P$，假设当前的路径是$P'$，我们可以先假设只有一处连续相交的地方，设为$a\rightarrow b$​，假设P为$x\rightarrow p_a\rightarrow a\rightarrow b\rightarrow p_b\rightarrow y$

$P'$为$x\rightarrow p'_b\rightarrow b\rightarrow a\rightarrow p_a'\rightarrow y$

既然当前的路径$P'$是增广路径，那么需要将路径上所有边翻转，那么

可以构造如下两条路径
$$
x\rightarrow p_a\rightarrow a\rightarrow p'_a\rightarrow y\\
x\rightarrow p'_b\rightarrow b\rightarrow p_a\rightarrow y
$$
其他情况也是同理

显然在容量为1的网络中，最小割的大小就是等于$X,Y$​集合之间的边，也就是等于不相交路径数量，因为一旦存在，那么说明$X\rightarrow Y$​之间还存在一条没有满流的边，那么说明残量网络上还可以找到一条增广路，那么当前的流就是不是最大流，导出了矛盾


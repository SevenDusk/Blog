# C. Inversions

首先将$inv(p)^k$转化为$\sum\limits_{i=0}^k \left\{\begin{matrix}k\\i\end{matrix}\right\} inv(p)^{\underline{i}}$

其中$inv(p)^{\underline{i}}$的组合意义就是在所有$p$的逆序对中选出不同的$i$个逆序对的方案数

考虑到由于需要选出的逆序对数量很少，那么显然涉及到的位置只有最多$2k$个，那么如果我们固定涉及到的位置的话，记$f(c)$表示选出的逆序对涉及到的位置有$c$个并且确定这$c$个位置之间相对大小关系的方案数

那么最终答案
$$
\sum\limits_{c}\binom{n}{c}^2(n-c)!cal(c)
$$
主要是如何求出$cal(c)$，我们可以考虑这样的一个过程，假设我们固定c，然后从大到小插入元素，那么插入一次元素形成的逆序对就是插入位置前面存在多少个元素

那么就可以考虑一个DP，设$f(n,m)$表示我们插入了$n$个元素，在其中选择了$m$个逆序对方案数

转移的时候考虑枚举当前选择了多少个逆序对
$$
f(n,m)=\sum\limits_{i=0}^m f(n-1,m-i)\sum\limits_{j=i}^{n-1} \binom{j}{i}=\sum\limits_{i=0}^m f(n-1,m-i)\binom{n}{i+1}
$$
那么我们可以通过FFT优化到$O(n^2\log n)$

但是这样做其实是会重复计算的，就是我们要求的是**涉及**到的位置，我们在转移的过程中可能会产生没有被选择到的逆序对涉及到的位置

那么考虑类似进行二项式反演，设$g(n,m)$表示恰好涉及到$n$个位置，在其中选择了$m$个逆序对的方案数
$$
f(n,m)=\sum\limits_{i=0}^n \binom{n}{i}^2 (n-i)! g(i,m)\\
\frac{f(n,m)}{(n!)^2}=\sum\limits_{i=0}^n (n-i)!\frac{g(i,m)}{(i!)^2}
$$
记$F_m(x)=\sum\limits_i f(i,m)\frac{x^i}{(i!)^2},G_m(x)=\sum\limits_i g(i,m)\frac{x^i}{(i!)^2}$

那么我们有
$$
F_m(x)=e^xG_m(x)\\
G_m(x)=e^{-x}F_m(x)
$$
那么
$$
g(n,m)=\sum\limits_{i=0}^n (-1)^{n-i}\binom{n}{i}^2(n-i)!f(i,m)
$$
其中
$$
cal(c)=\sum\limits_{i} \left\{\begin{matrix}n\\i\end{matrix}\right\} g(c,i)
$$

$$
\begin{align}
&\sum\limits_{i=0}^{lim}\binom{n}{i}^2 (n-i)!\sum\limits_{j=0}^m \left\{\begin{matrix}m\\j\end{matrix}\right\}j!\sum\limits_{k=0}^i (-1)^{i-k}\binom{i}{k}^2(i-k)!f(k,j)\\
&=\sum\limits_{j=0}^m \left\{\begin{matrix}m\\j\end{matrix}\right\} j!\sum\limits_{k=0}^{lim} f(k,j)\sum\limits_{i=k}^{lim} \binom{n}{i}^2(n-i)!\binom{i}{k}^2(i-k)!(-1)^{i-k}
\end{align}
$$

预处理最后一个和式，最终复杂度就是$O(m^2)$，需要分段打表预处理n!，可以发现n-2*m>=mod 就一定答案为0

# F. Kill All Termites

首先如果对于若干节点下毒，那么会将整个树分成若干个连通块，并且如果某一个连通块中存在一个节点出发，可以永远不走出这个连通块，那么这个下毒的方案就是不合法的

由于叶子节点十分特殊，可以来回走，那么可以发现对于一个连通块来说如果存在两个叶子节点，那么可以在两个叶子节点之间不断来回走，那么就是不合法的

那么考虑只有一个叶子节点的情况，由于树上两个点之间的路径只有一条，那么无论从哪一个点出发走到叶子之后就不能再走回来了，那么相当于将这个叶子节点给删除掉了，那么对于没有叶子节点的连通块显然会走出这个连通块

那么就得到了一个判定某一个连通块是否合法的条件，就是这个连通块存在的叶子数量$\leq 1$

那么可以设$dp(x,0/1)$表示当前包含x的连通块中含有0/1个叶子节点最少下毒节点数量

转移直接将儿子的DP对于第二维进行背包合并即可，然后需要注意可以对于这个节点进行下毒，那么$1+\sum \min(dp(u,0),dp(u,1))$可以更新$dp(x,0)$

时间复杂度$O(n)$

# G.Maximal Subsequence

首先如果某一个位置不可能出现在LIS中，那么显然可以保留这个位置

唯一需要考虑的位置就是可能出现在LIS中的位置，首先我们可以处理出来$L_i$表示从$i$这个位置开始存在的最长上升序列长度

然后相当于给所有位置进行分层，然后在相邻两层之间进行连边，如果$i\rightarrow j$，那么需要满足$L_j=L_i-1,a_j>a_i$，然后考虑不选哪一些位置，那么显然没有选中的位置在这张图上形成了一个割集，相当于去除这些点之后$L_i=1,L_i=k$两层的点是不连通的

那么如果将每一个位置进行拆点，限制流量为1，那么相当于就是在这个图上求最小割，由于最小割等于最大流，那么相当于就是求最多存在多少个长度为$k$的LIS并且两两之间不交

这个问题就是[COCI 2021 Volontiranje](https://www.luogu.com.cn/problem/P7931)

同样还是分层，可以发现同一层里面，如果按照下标进行排序，那么有$i<j,a_i>a_j$，可以考虑贪心的进行选取，首先可以发现的是，如果我们将$(i,a_i)$画在坐标系上，然后我们在选出的序列之间进行连边的话，如果存在交叉的情况，显然我们可以交换交叉的两端使得画出的线段是不交叉的

也就是如果我们每一次选取下一层比当前位置大的第一个位置一定是最优的，这样留给下一条线段的选择余地最大，同样我们在选取第一个位置的时候也是从大到小进行选择

并且我们在选取过程中需要dfs出来第一个合法的线段，如果当前线段不合法，那么将当前位置删除，显然这个位置之后不可能再被选择到

这样的时间复杂度$O(n\log n)$

#  J. Two Permutations

首先$\max(a,b)=\frac{a+b+|a-b|}{2}$

那么
$$
\sum \max(p_i,q_i)=\sum \frac{p_i+q_i+|p_i-q_i|}{2}=k
$$
那么其要求就可以转化为
$$
\sum |p_i-q_i|=2k-n(n+1)
$$
那么考虑一个匹配问题，左右侧点数都是$n$，然后考虑从1到n进行扫描线，过程中记录再前$i-1$个位置还有多少个位置没有进行匹配（显然左右侧未进行匹配的点数相同），当需要加入一个节点的时候，讨论这个节点是向之前匹配还是对应位置匹配还是向后匹配，那么就可以确定这个i对于上面式子贡献的正负号

那么设$dp(i,j,k)$表示以上问题的答案并且式子的和为$k$
$$
dp(i-1,j,k)\times j^2\rightarrow dp(i,j-1,k+2i)\\
dp(i-1,j,k)\rightarrow dp(i,j+1,k-2i)\\
dp(i-1,j,k)\times 2j\rightarrow dp(i,j,k)\\
dp(i-1,j,k)\rightarrow dp(i,j,k)
$$
以上四个转移分别表示当前加入的两个i节点

- 匹配的都是之前的节点
- 匹配的都是之后的节点
- 其中一个匹配之前的节点，另一个匹配之后的节点
- 互相匹配

最终答案$dp(n,0,2k-n(n+1))$

时间复杂度$O(n^4 )$

# H. Aidana and Pita

考虑进行折半搜索，假设搜索出来的三个人的和为$(a_1,b_1,c_1)$，第二部分搜索出来的和为$(a_2,b_2,c_2)$

### 比赛的做法

由于题目要求的是极差，关系到大小关系，但是我们并不知道三元组加起来每一维度上数字之间的大小关系，就很难进行处理，考虑最大值和最小值和绝对值是有关系的，那么考虑转化为绝对值的形式

假设有三个数$x,y,z$，那么一定存在一个数夹在最大值和最小值之间，而绝对值函数反应的是两个点之间的距离，极差反映的是最远点和最近点之间的距离

那么$|x-y|+|x-z|+|y-z|=2(\max(a,b,c)-\min(a,b,c))$

那么我们要最小化
$$
|a_1+a_2-b_1-b_2|+|b_1+b_2-c_1-c_2|+|a_1+a_2-c_1-c_2|
$$
令$x_1=a_1-b_1,y_1=b_1-c_1,z_1=a_1-c_1$

那么我们需要最小化的是
$$
|x_1-x_2|+|y_1-y_2|+|z_1-z_2|
$$
那么相当于给定两个三维空间中的点集需要求出两个点集中最近的距离（曼哈顿距离）

那么可以讨论三个绝对值的取值，那么剩下的是三维偏序问题，用cdq分治可以在$O(n\log ^2n )$的时间内解决，但是需要乘上8的常数

并且总点数为$2e6$，通过强制$x_1\leq y_1\leq z_1$可以将总点数缩减至$7e5$，但是还是难以通过，大概需要3s左右，时限2s

### 正确做法

事实上如果我们钦定$a_1+a_2\leq b_1+b_2\leq c_1+c_2$事实上在所有情况的组合中这样是一定存在的，并且覆盖了所有情况（其他情况都是这种情况的排列）

那么我们只需要求出满足这样条件的三元组对即可
$$
a_1-b_1\leq -(a_2-b_2)\\
b_1-c_1\leq -(b_2-c_2)
$$
那么令$x_1=a_1-b_1,y_1=b_1-c_1,x_2=b_2-a_2,y_2=c_2-b_2$

那么需要求出$x_1\leq x_2,y_1\leq y_2$并且最小化$x_2+y_2-x_1-y_1$即可，那么相当于一个二维偏序问题，用树状数组即可在$O(n\log n)$时间内解决
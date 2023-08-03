# **01 Balanced**

## 题目大意

给定m个限制$[l,r]$，要求构造长度为n的字典序最小01序列，使得

$[l,r]$中的01出现次数相同
$$
n\leq 10^6,m\leq 4\times 10^5
$$

## 算法讨论

### Solution 1

考虑1的前缀和$s_i$

需要满足以下的条件
$$
0\leq s_i-s_{i-1}\leq 1\\
s_r-s_{l-1}\leq \frac{r-l+1}{2}\\
s_r-s_{l-1}\geq \frac{r-l+1}{2}
$$
根据这些不等式需要建出差分约束系统

由于图中是存在负环的，那么无法跑dij只能用spfa，但是可能会被卡到$O(nm)$，那么需要对spfa进行优化

考虑用SLF+容错+mcfx优化，这是因为这张图的边权和比较小只有$O(n)$级别，其中一大部分的正负权边是不需要考虑的，那么我们可以取容错系数为$\sqrt n$

（其中SLF+容错指得是当入队的时候，如果比当前队首权值大$\sqrt n$那么加入队尾，否则加入队首，然后mcfx优化是指在一个点第$[2,\sqrt n]$次被枚举到的时候直接加入队首，否则加入队尾）

其中mcfx可以增加能被卡掉的权值和大小（比较玄学）

那么理论的复杂度就是$O((n+m)\sqrt n)$

## Solution 2

注意到如果我们可以将所有边权变成正数的话，那么就可以用dij来跑

考虑记$s_i$表示前i个位置0的个数-1的个数

那么有
$$
|s_i-s_{i-1}|=1\\
s_r-s_{l-1}\leq 0\\
s_r-s_{l-1}\geq 0
$$
那么连的边权都是0或者1，那么可以直接使用0/1bfs或者dij即可

https://atcoder.jp/contests/agc056/submissions/27691613

# **Cheese**

## 题目大意

给定一个环，环上有n个节点，对于$0\leq i<n$来说，在$i+0.5$处有一只老鼠，现在要扔出n-1个食物，扔到第$i$坐标的概率为$A_i$

然后不断在环上转，如果当前老鼠没有吃过食物，那么有$\frac{1}{2}$的概率吃掉这个食物，求最终第k个老鼠没有吃到食物的概率
$$
n\leq 40
$$

## 算法讨论

首先考虑对于一个固定扔出食物的坐标来说如何进行处理

如果我们可以求出在整个过程之中每一个老鼠面前经过的食物次数，那么对于一个最终吃到食物的老鼠来说，在面前经过的这些食物中存在一个吃掉的，那么假设经过了cnt次，那么概率就是$1-(\frac{1}{2})^{cnt}$，而对于一个没有吃过食物的老鼠来说，其概率就是$(\frac{1}{2})^{cnt}$

而对于那些中途不够的吃的情况，也就是cnt<0的情况，概率就是0

我们假设$r_i$表示扔到第i个坐标的个数，然后记$cnt_i$表示经过第i只老鼠的食物次数

如果我们可以确定其中一个位置经过的食物次数，就可以推出其他所有位置食物的经过次数

那么可以考虑当前求出的概率是第$n-1$只老鼠没有吃到的概率，然后枚举经过第n-1只老鼠之后那个位置的食物次数x

那么对于其中第i只老鼠来说，前面的老鼠最终都是要吃掉一个食物的，然后对于前面扔下的食物也是需要加入到经过当前老鼠的位置上的

那么有
$$
cnt_i=x-i+\sum\limits_{j=0}^{i}r_j
$$
然后按照上面叙述过程计算概率

需要注意的是，对于一个合法存在的cnt数组来说，上面的过程是合法的，而现在我们枚举的是x，x是会存在可能不合法的情况，就是会多出来若干个不是从任何一个位置扔下，也没有被任何一只老鼠吃掉的食物，这种情况是不合法的，但是实际上对于一个固定的x，我们在没有具体每一个食物扔下的位置的信息的时候，无法判断哪些情况是不合法的

**这时候就不能单独只看一个x，因为我们枚举x，最终要算出所有x的和**

可以发现的是，一旦存在这样一个不合法的食物，其在$n-1$这个位置会多算$\frac{1}{2}$的贡献，其他位置都是不影响的

那么如果假设一个合法方案的概率是$p$

那么所有这样不合法方案累加的概率就是
$$
p\sum\limits_{i\geq 0} \frac{1}{2^i}=2p
$$
显然所有不可能的方案数都是在对x求和中枚举到，那么只要按照存在不合法方案数的计算过程计算，最终答案除以2即可

显然最终贡献的情况就是关于$\frac{1}{2^x}$的一个多项式，我们可以通过DP求出这个多项式

设$dp(i,j)$表示当前考虑到第i个位置，其中已经扔下了j个食物的多项式

转移的时候枚举当前位置要扔下多少个食物，然后乘上对应的概率$A_i^k$以及方案数$\binom{j+k}{j}$，将贡献拆成1次多项式然后乘上去即可

假设最终得到的多项式为$F(x)$

那么所求就是
$$
\begin{align}
&\sum\limits_{x\geq 0} F(x)\\
&=\sum\limits_{x\geq 0} \sum\limits_{i} f_i x^i\\
&=\sum\limits_{i} f_i \sum\limits_{x\geq 0} (\frac{1}{2^i})^x\\
&=\sum\limits_{i} f_i\frac{1}{1-\frac{1}{2^i}}
\end{align}
$$
然后不断将A旋转，使得每一个位置都变成n-1一次计算出答案即可

时间复杂度$O(n^5)$

# **Range Argmax**

## 题目大意

给定m个限制$[l,r]$，要求存在多少个x序列使得存在一个排列$p$

满足对于任意$i$，使得$\max(p_l,...,p_r)=p_{x_i}$
$$
n\leq 300
$$

## 算法讨论

首先可以显然的得出一个x序列合法的充要条件

对于任意两个$i,j$来说，如果$x_i,x_j\in [L_i,R_i]\cap [L_j,R_j]$，那么一定有$x_i=x_j$

然后像这样的题目关于$max$有关的，可以考虑将$n,...,1$这些数依次插入到排列中，那么一个区间的$x_i$就是这个区间内第一个被插入的位置

那么相当于在插入一个数之后，将所有包含这个位置的区间删除，然后进行接下来的插入，可以发现这样一定是满足上面这个条件的

但是如果直接将这样的插入方式进行计数，对于同一个$x$序列是存在不止一种顺序进行插入的

那么就需要一种特定的插入方式，将所有合法插入方式变成只有一种合法插入方式

由于插入一个数之后，需要将所有包含这个位置的区间删除，那么对于一个x序列，如果某一个位置$pos$上所有区间的$x_i$都是等于$pos$，那么说明这个当前这个数可以插入当前这个位置

显然对于一个时刻来说，可能存在多个可以插入的位置，那么就选择最左边的那个进行插入即可

那么剩下的事就是如何在模拟这个插入过程中统计方案数，在插入的过程中考虑$[l,r]$这个区间，其中l-1,r+1要门事边界要么被插入过了

考虑枚举当前这个区间内插入的位置$p$，显然对于右边的情况，直接递归求解$[p+1,r]$的问题即可，然后对于左边的情况就有点复杂

由于我们强制需要插入所有可以插入位置中最靠左的那个位置，那么左边区间是不能存在当前可以插入的位置的，假设覆盖$p$的所有区间中左端点最小为$MIN$

那么递归到$[l,p-1]$区间的时候，插入的位置必须在$[MIN,p-1]$之间，否则就会与之前的过程矛盾

那么设$dp(l,r,k)$表示当前区间为$[l,r]$，可以插入的位置为$[k,r]$的方案数

转移的时候需要前缀和优化即可

时间复杂度$O(n^3)$
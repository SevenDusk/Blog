# NOMURA Programming Competition 2020 F Sorting Game

这题值得一写

首先需要考虑对于怎样的序列，第二个人一定能排好序

由于排序实际上就是将序列中逆序对的个数降为$0$，并且对于任意一个逆序对，都要经过一次交换

那么第二个人可以排好序的充分必要条件就是

不存在$i<j$，满足存在一组删位方式使得$a[i]>a[j]$并且$a[i]$与$a[j]$二进制位不同的个数$>1$

那么第一个人就是要想办法找到这样一对$(i,j)$

由于将一位全部变为$0$，只会使序列中可以交换的对数不减，那么第一个人一定是利用这个操作改变逆序对，那么进行操作的位一定是从高往低位进行操作（反证法可证）

那么考虑判断两个下标上的数$i<j$会不会使上面那个条件成立，考虑从高位开始考虑，设$a[i]$当前位为$c_i$，$a[j]$当前位为$c_j$

如果$c_i=c_j$那么删去这位不影响

如果$c_i=0,c_j=1$那么如果不删去这一位，就一定不会产生逆序对，那么也是删去

如果$c_i=1,c_j=0$那么产生了逆序对，如果低位部分不相同，那么这个序列就是不合法序列，否则删去继续判断

根据上面的过程，考虑设$dp[i][j]$表示当前考虑到第$i$位，序列中有$j$个元素的合法序列数

考虑如果第$i$位上，是$000...0111...1$这样的序列，那么直接删去即可，$dp[i][j]=(j+1)dp[i-1][j]$

否则如果存在$10$子序列，那么低位一定都要相同，并且$1$的位置和$0$的位置之间的数的低位，也要相同（这样可以将这些数看作同一个数）

相当于$0...001...011...1$这样的序列，考虑枚举将中间一段缩起来之后的长度$dp[i][j]=\sum\limits_{k=1}^{j-1}k2^{j-k-1}dp[i-1][k]$

计算的时候统计前缀和即可

2021.3.1 21:45

# CODE FESTIVAL 2017 Final F **Distribute Numbers**

也是道好题

首先需要找出$n$与$k$之间的关系

考虑由于有对于每一个数都出现的$k$次，由于任意一对纸张都只有一个相同的数，那么这些有序对都是在这$n$组$k$个数中产生的

那么可以列出等式$n\frac{k(k-1)}{2}=\frac{n(n-1)}{2}$

那么$n=k(k-1)+1$

然后考虑构造方案

可以先画出一个$(k-1)*(k-1)$的矩阵A，那么还剩下$k$个数，考虑每一张纸上的第一个数在这$k+1$个数中

首先第一张纸写下这$k$个数，那么还剩$k*(k-1)$张纸，$k$个数中每一个数开头都有$k-1$张纸

首先第一个数，每一张纸都写$A$中每一列

之后$k-1$个数，标号为$[1,k-1]$，对于第i个数，其第j张纸所写的数为$A_{j,0},A_{(j+i)\%k,1},A_{(j+2i)\%k,2}...$

如果$k-1$为质数，这样构造一定合法，考虑从当前列跳到下一列，一定都是对应其他数的不同起始点的路径

2021.3.2 22:41

# AGC52B Tree Edges XOR

思路有点清奇

其主要思想就是，将题目条件转化到简单操作上来，然后分析简单操作

然后这道题操作边的影响很复杂，需要考虑的是将边的操作转化到点上来

这道题就是对于每一个点需要构造一个函数$f(x)$，使得$f(u)$ XOR $f(v)=w_{u,v}$

然后构造的是，任意选定一个点，求出这个点到$x$的路径上的异或和即为$f(x)$，显然满足条件，并且一次操作的结果就是交换操作那条边两个端点的函数$f$，因为操作进行后，穿过被影响的区域，异或和是不变的，那么只要分析被影响的区域

那么只要判断$w_i^1$和$w_i^2$分别得到的函数是不是互为排列

考虑先假定从$1$号节点出发，得到两个距离数组，现在假设出发点从$1$号点不断向下移动，可以发现是对于全体距离数组异或边权，然后对于这个数，从高位向低位确定即可（这部分不难）

# AGC29F Construction of a tree

居然做出来了

首先需要考虑从一棵树是通过合并不同联通块所得到的

考虑从全集不断分裂联通块

最初$s=\{1,2,3...,n\}$，对于当前还没有选到的集合$E_i,|E_i\cap S|\geq 2$，每次操作选出一个没有选到的集合，将$s$分裂成两个集合，使得$E_i$与这两个集合的交至少为$1$

然后可以发现，如果只分裂出一个节点，一定比分裂出两个大小都$>1$的集合更优

那么相当于是，选取$x\in E_i$令$s'=s-\{x\}$并且满足上述条件

于是这是一个匹配问题，每一个节点对应着删除其的集合，那么建出二分图，如果跑出的最大匹配$<n-1$那么一定无解

由于在删除的过程中需要满足所有还没有选到的集合与$s$的交都是大于等于$2$，也就是说在匹配某一个集合的时候，与其相连的节点至少有两个

对于匹配完之后的二分图，建立有向边，如果是匹配边集合向节点连边，否则节点向集合连边，就是跑完网络流之后的残量网络

<img src="D:\Blog\image\graph.png" alt="graph" style="zoom: 67%;" />

由于节点数比集合数多一个，那么就从多出来的那个节点$x$出发

首先直接相连的集合一定是可以进行匹配的，因为这些集合在进行匹配的时候可以将匹配点$y$与$x$连边，那么现在$y$也可以去跟其他进行连边，以此类推

那么只要判断是否存在一个节点出发，可以遍历到所有节点，那么tarjan缩点，拓扑判断即

# AGC24F Simple Subsequence Problem

首先考虑设每一个串的价值为在s中作为子序列的最多个数

那么如果将$s$中每一个元素的所有子序列都加上$1$，那么对于得到的就是每一个串的价值

考虑DP，首先需要找出唯一确定某一$s$中元素所有本质不同子序列的方法

考虑在匹配过程中，每一次匹配都是找下标最小的位置去进行匹配，那么这样的匹配方法一定唯一

设$dp[s][i]$表示s的最后i位都是已经匹配的子序列，前面都是还未进行匹配的原串的字符的价值之和

考虑转移，每一次可以从前面转移最后的$1$或者$0$，并将界限加$1$

然后当前面部分是空串的时候的$dp$值就是后面这个串的价值

复杂度$O(n2^n)$

# AGC52C Nondivisible Prefix Sums

不愧是AGC

首先可以发现，如果某一个序列$\{a_i\}$合法，那么$\sum a_i\not\equiv 0(\bmod P)$

考虑**从前往后**一个一个加数，记当前的前缀和为$S$，并且在每一次加数后，需要保证S不为P的倍数，那么可以发现的是如果当前没有选到的$\{a_i\}$中存在两种不同的数，那么一定可以将选出一个数使得其不为0

那么不合法情况只有当前只存在一种数的情况，这个可以联想到众数，

考虑一个转化，对于任意一个合法的序列，设其出现最多的数为$r$，那么序列$\{a_ir^{-1}\}$也仍然是一个合法的序列，并且当前出现最多的数就是$1$

那么需要尽可能减少最后$1$的剩余

**那么考虑一个构造**
$$
\overbrace{1,1,\dots,1}^{p-1},a_{t_1},\overbrace{1,1,\dots,1}^{p-a_{t_1}},a_{t_2},\overbrace{1,1,\dots,1}^{p-a_{t_2}},a_{t_3},\dots
$$
其中$a_{t_i}$是序列中$>1$的数

那么猜一个结论，设$c_i$表示序列中$i$出现的次数，那么一个序列合法的充要条件是
$$
c_1\leq \sum\limits[a_i>1](p-a_i)+p-1
$$
充分性从上面的构造可以看出来

考虑证明必要性，设
$$
c_1> \sum\limits[a_i>1](p-a_i)+p-1
\Leftrightarrow \sum a_i\geq p(1+\sum\limits_{i>1}c_i)
$$
考虑不断向后面加入，那么跨越$P$的次数为$1+\sum\limits_{i>1}c_i$

每一次跨越$P$，都需要消耗一个$>1$的数，数的数量$\sum\limits_{i>1}c_i$严格小于跨越的数量，那么导出矛盾

那么只要算出总和不为$0$的序列，再减去不合法序列即可

由于一个序列不合法，需要满足
$$
c_1>\sum\limits_{i>1}(p-i)c_i +p-1 >\sum\limits_{i>1}c_i
$$
那么显然$c_1$一定是序列的严格众数，那么只要算出$>1$的数组成的序列，然后插入1即可

设$dp[i][j]$表示当前序列中有$i$个$>1$的数，并且其对限制的贡献为$j$方案数
$$
dp[i][j]=\sum\limits_{k=1}^{\min(p-2,j)} dp[i-1][j-k],j\leq n-i-p
$$
插入1的方案数为$[j-n+i \not\equiv 0(\bmod p)]dp[i][j]\binom{n}{i}(p-1)$

其中$p-1$是将$1$还原成原来的数

# AGC19E Shuffle and Swap

~~我现在只会做远古AGC的题。。~~

首先考虑将第一个串中$1$的位置集合记作$A$，将第二个串中$1$的位置记作$B$，令$S=A\cap B,T=C_uS$

那么每一次都可以选择一个$A$中的数$x$和$B$中的数$y$，交换第一个串中$x$和$y$的位置

根据$x$是否属于$S$，可以得到如下性质

若$x\in S$那么第一个串和第二个串中下标为$x$的位置上都为1

若$x\notin S$那么第一个串下标为$x$的为1，第二个串下标为$x$的为0

若$y\notin S$那么第一个串下标为$y$的为0，第二个串下标为$y$的为1

考虑以下四种情况

- $x\in S,y\in S$那么直接交换不会影响串的形态
- $x\notin S,y\in S$那么串的形态仍然不会发生改变，但是由于$x\notin S$，那么第二串中$x$的位置为$0$，而第一个串中$x$为$1$，并且之后不会再改变x这个位置，那么这种操作不合法
- $x\in S,y\notin S$其实就相当于将$x$移出$S$集合，并且将$x$加入$T$，然后将$y$从$T$中删除
- $x\in S,y\in S$
  - 若$x=y$那么直接将$x$从$S$中删除
  - 若$x\neq y$那么就相当于从$S$中删除$x,y$，但是会产生两个另外一个串对应的下标，并且这两个下标只能用于该操作

根据上面的情况分析，注意到如果硬将三种合法操作都在一个DP中刻画出来，会发现最后一种操作非常特殊，三维的DP式子也会因为这个操作的存在而变得非常复杂

可以发现，这个操作对答案的贡献只跟进行操作的次数有关，那么考虑枚举该种操作次数$k$

接下来DP出前两个操作的方案数，设$dp(i,j)$表示当前$|S|=i,|T|=j$

那么可以得到刷表递推式，然后将改递推式的起点和终点交换，会发现得到的答案仍然相同
$$
dp(i,j)=ij*dp(i-1,j)+j^2*dp(i,j-1)
$$
答案为
$$
\sum\limits_{k} dp(|S|-k,|T|)(k!)^2\binom{|S|}{k}\binom{|S|+|T|}{k}
$$
$k$为枚举第三种操作方案，由于其之间可以任意排列，方案数为$(k!)^2$，需要从$|S|$个数里面选出$k$个，并且操作顺序中选出$k$个位置

复杂度$O(n^2)$可以用生成函数继续优化至$O(n\log n)$

# AGC41D Problem Scores

首先可以发现，如果满足$k=\lfloor\frac{n-1}{2}\rfloor$的情况就一定满足条件

考虑刻画这种情况，假设$A_{k+1}=B$
$$
d_i=\left\{
\begin{aligned}
A_{k+1}-A_i&(i\leq k+1)\\
A_i-A_{k+1}&(k+1<i\leq n)
\end{aligned}
\right.
$$

$$
\begin{align}
B(k+1)-\sum\limits_{i=1}^{k}d_i&>BK+\sum\limits_{i=k+2}^n d_i\\
B&>\sum\limits_{i=1}^kd_i+\sum\limits_{i=k+2}^n d_i
\end{align}
$$
由于对于$i\leq k$来说$0\leq d_i<B$，$i\geq k+2$来说$0 \leq d_i\leq n-B$

那么就转化为一个带有个数限制的完全背包，我们可以先枚举$B$

考虑差分之后的贡献，那么每一个位置相当于贡献后缀长度，那么转化为$1,2,...k$中最多选$B-1$个数，后半部分也是同理

注意到前面部分，最多选$B-1$个数的限制是没有用的，那么直接做完全背包，处理出$f(i)$表示$d$的和为$i$的方案数

考虑后面部分，设$dp(i,j)$表示选了$i$个数，$d$的和为$j$的方案数，为了避免背包合并的复杂度瓶颈，那么dp的初始条件可以为$dp(0,i)=f(i)$

可以从大到小枚举$p$，按照完全背包的思路更新$dp(i,j)=dp(i,j)+dp(i-1,j-p)$，注意到如果一个状态合法，一定有$ip\leq j$，那么转移的复杂度为$O(n^2\log n)$

最后统计答案即可，注意讨论一下奇数和偶数的情况

# ARC78F Mole and Abandoned Mine

考虑最后合法的情况，图一定是一条$1$到$n$的链，并且链上的节点，挂着若干节点，并且不直接连向其他链上的节点或者其他节点上挂着的节点

可以将题意转化为保留边权值最大化

考虑对这条链进行DP，设$dp(S,x)$表示当前已经选入S集合的点，并且链上最后一个点是$x$的边权和最大值

转移的时候考虑枚举$y$和$T$，其中$S\cap T=\empty,y\in T$，那么$dp(S,x)+f(T)+w(x,y)\rightarrow dp(S\cup T,y)$

其中$f(T)$表示$T$集合形成一个链上的节点的最大边权和，转移比较简单，注意不联通情况需要赋为$-inf$

# AGC38E Gachapon

首先可以发现，题目所求是所有的$i$都满足条件的期望，相当于是一个$\max$的形式

考虑$min-max$容斥
$$
\max(S)=\sum\limits_{T\neq \empty} (-1)^{|T|-1}\min(T)
$$
其中$\min(T)$的意义是$T$中有一个满足条件的期望

根据期望的定义可知$E=\sum\limits_{i\geq 1} iPr[x= i]=\sum\limits_{i\geq 0}Pr[x> i]$

对于$i\in T$，可以先取出$B_i-1$个进行排列，然后对于所有$i\notin T$看作同一个物品，并且有无限多个，选出一个的概率为$p_0$。如果从这些物品选出$k$个进行排列得到的概率就是$Pr[x>k]$的概率

设$g(i)=\sum\limits_{k=0}^{B_i-1}\frac{p_i^k}{k!}x^k$，$F_T(x)=\prod\limits_{i\in T} g(i)=\sum\limits_i \frac{f_i}{i!}x^i$

那么计算出$F_T(x)$，那么就可以计算出只考虑$i\in T$物品的情况，那么接下来考虑剩下那个无限个物品如何插入生成序列中，那么贡献就为
$$
\begin{align}
&=\sum\limits_i f_i\sum\limits_{j\geq 0} \binom{i+j}{j}p_0^j\\
&=\sum\limits_i f_i\frac{1}{(1-p_0)^{i+1}}
\end{align}
$$
那么接下来就是利用DP统计容斥的结果，由于计算贡献的过程跟多项式有关，那么DP的值也需要记录多项式

设$H(i,j)$表示考虑到前$i$个数，$\sum\limits_{i\in T}A_i=j$所有情况的多项式之和
$$
H(i,j)=H(i-1,j)-g_i(x)H(i-1,j-A_i)
$$
直接进行转移是$O(n^4)$的，由于$\sum B_i\leq 400$，也就是说所有$H(i,j)$代表的多项式次数不会高于400，那么可以将$H$转为点值形式进行转移，那么复杂度为$O(n^3)$

# Code-Festival-2017-QualC F Three Gluttons

首先可以注意到对于C序列，我们只要关心选出来的数，将其他数填入得到的排列数都是固定，与选出来的数无关

具体的，对于$\frac{n}{3}$个选出来的数来说，每一个数后面都可以插入两个其他的数，从后往前考虑，假设考虑到从后往前第$i$个位置，那么已经确定$3i-3$个位置，要插入2个数，方案数为$(3i-2)(3i-1)$

那么总方案数为$G=\prod\limits_{i=1}^{\frac{n}{3}} (3i-2)(3i-1)$

现在只需要考虑有多少种选出来的方案

假设当前第$k$次选择，选择了$A_i,B_j,x_k$，那么$A_i,B_j$在$A$的$[1,i]$和$B$的$[1,j]$中只出现了一次，而$x$没有出现，由于$x$序列元素互不相同，那么当前的方案数为
$$
f(i,j,k)=n-3(\frac{n}{3}-k)-|\{A_1,A_2,...,A_i\}\cap\{B_1,B_2,...,B_j\}|
$$
那么可以用DP进行统计，复杂度$O(n^3)$

一开始想，如果确定了$[1,i],[1,j]$​的位置，那么就可以知道下一次会到那里进行一轮操作，对于C序列来说先不进行分配，当进行操作的时候再进行分配，这样也可以用DP进行统计，但是这样会算重，很难解决，主要差别是在对于C来说是当场进行分配还是后面操作的时候才分配，一开始没有想到当前决策的方案可以计算出来

# ARC125C LIS to Original Sequence

像这种题应该想到递归构造，感觉我对这个递归构造很不熟悉。。。

首先由于是排列，如果我们处理好第一个元素，然后将其他元素离散化，就可以变成一个规模更小的问题

那么首先考虑一些特殊情况

如果$k=1$，那么唯一一种合法的排列就是$n,n-1,...,1$

如果$a_1=1$，那么显然第一个位置放$1$最优，那么就转化为$n'=n-1,A=\{a_2-1,a_3-1,...,a_k-1\}$的问题

那么需要考虑的就是$a_1>1$​的情况，由于需要构造的是字典序最小的情况，可以构造$a_1,1$的情况，显然这样是合法的，那么递归下去构造就可以了

# AGC10E Rearranging

## 题目大意

现在有$n$个数，每一个数为$A_i$，第一个人可以任意重新排列这些数，第二个人可以任意交换两个相邻的互质的数，第一个人先操作，第一个人要得到的序列字典序最小，第二个人要得到的序列字典序最大

求出最终的序列
$$
n\leq 2000
$$

## 算法讨论

首先给互相gcd不等于1的点之间连边，第一个人相当于就是给这个无向图定向，然后第二个人就会进行拓扑排序，并且贪心的选择当前可以选择的最小的数

考虑从前往后枚举当前这一位是哪一个数，如果这个数代表的节点为$x$

那么$x$连向的节点都是没有问题的，但对于$x$没有连边的点，并且这个点的数值比$a_x$大，那么就说明这个点在当前时刻时至少应该有一个入边，那么我们用$tag(x)$来记录某一个点是否需要强制选一个入边

显然我们当前位置选择的$x$，$tag(x)\neq 1$，选定之后可以给其连向的节点$tag=0$，并且如果某一个联通块内所有节点的tag都为1，那么显然是会出现环的情况，那么就不合法

那么每一次选择节点的时候需要保证，选择过后不会出现某一个联通块全是$tag=1$的情况，考虑求出每一个联通块中数值最小$tag=0$的节点，那么我们选择的一定是求出节点中数值最大的那个

至于维护联通块就是先预处理每一个数的质因数，然后利用并查集维护即可

时间复杂度$O(n^2\log n)$

# CF618F Double Knapsack

脑筋急转弯题，虽然第一眼就看出应该是鸽巢原理的题……

首先对$A$序列做前缀和$a$，$B$序列做前缀和$b$，假设$a_n\geq b_n$

那么考虑对于每一个$i\in [0,n]$找到最大的$j$满足$b_j\leq a_i$
$$
a_i<b_j+B_{j+1}\leq b_j+n\Leftrightarrow a_i-b_j< n
$$
那么$a_i-b_j$的取值只有n个，而i总共有n+1个，那么一定存在$i>i',j>j'$使得$a_i-b_j=a_{i'}-b_{j'}\Leftrightarrow a_i-a_{i'}=b_j-b_{j'}$

那么区间$(i,i']$和$(j,j']$中所有数就是符合条件的一组解

# CF739E Gosha is hunting

Solution 1:费用流

首先期望等于，每一个能被捕获的概率之和

如果某一个同时使用了两种球，那么其被捕获的概率为$p_i+u_i-p_iu_i$

注意到，单独使用某一个球的概率为$p_i$或$u_i$，那么在加入第二种球的时候，需要减去一个定值$p_iu_i$

考虑费用流

建立源点$S$，汇点$T$

$S$向$A$连费用为$0$，容量为$a$的边

$S$向$B$连费用为$0$，容量为$b$的边

然后$A$对于每一个$i$连费用为$p_i$，容量为$1$的边

$B$对于每一个$i$连费用为$u_i$，容量为$1$的边

$i$向$T$连费用为0，容量为1的边，再连费用为$-p_iu_i$，容量为1的边（这里是用到了如果某一条边的费用是分段递减的，那么可以拆边处理的技巧）

Solution 2:wqs二分

考虑$O(n^3)$的DP，设$dp[i][j][k]$表示前i个物品，选择了$j$个第一种球，$k$个第二种球

根据上面的费用流模型可以发现，对于后面两维，其代价函数是凸的，那么考虑两维上，相当于是类似金字塔的形状

考虑二维wqs二分，在第一维斜率确定下，再二分第二维情况，直到第二维选择的个数为b，同时保留最优解下第一维选择的个数，同样判定即可

# CF830D Singer House

考虑一条合法的路径，从叶子节点开始一层一层向上看，如果只保留当前层以下的节点，那么可以发现的是，这条路径会断成若干条**互不相交**的路径，再加入当前一层的节点的时候，会有一些路径被连接起来（最多2条），就是有点类似于线头DP的感觉，保证了不能重复经过某一个点，很符合线头DP的条件，都是对线段的延长和补全

那么考虑设$dp(i,j)$表示从下往上第$i$层子树内有$j$条互不相交的路径的方案数

考虑枚举左右儿子的路径条数，不妨设为$L,R$，令$k=dp(i-1,L)*dp(i-1,R)$

- 如果当前根不在路径中，$k\rightarrow dp(i,L+R)$
- 如果当前根单独作为一条路径，$k\rightarrow dp(i,L+R+1)$
- 如果当前根和左右子树某一条路径连上，那么可以连到这条路径的起点，也可以连到这条路径的终点，有2种方案，$2(L+R)k\rightarrow dp(i,L+R)$
- 如果当前根连接了两个不同的路径，那么也是存在两种方案，$2\binom{L+R}{2}k\rightarrow dp(i,L+R-1)$

最后答案为$dp(n,1)$，注意到每一次最多只能将总路径条数$-1$，而且最多进行$n$次合并，那么第二维的范围为$n$，那么总复杂度为$O(n^3)$

# CF1517F Reunion

很好的一道题

首先常规转化
$$
\begin{align}
E&=\sum\limits_{i=1}^{n-1} iPr[x=i]\\
&=\sum\limits_{i=1}^{n-1} Pr[x\geq i]\\
&=\sum\limits_{i=1}^{n-1} 1-Pr[x<i]
\end{align}
$$
然后不能只盯着那些可以到场的节点，如何那样做很难有一个好做法，后面得出的条件不是全集，是很难进行DP统计的

可以发现的是，一种方案的最大$r$如果小于$i$的话，记$S(x,i)$表示到$x$距离$\leq i$的点集，那么所有不可以到场的节点$S(x,i)$的并集就是全集

证明，如果存在一个可以到场的节点没有被覆盖到，那么这个点到最近的不可以到处的节点距离$>i$，那么$\leq i$的距离内都是可以到场的节点，跟假设不符

那么接下来就可以DP了，最简单的想法是设$dp(i,j)$表示$i$子树内最浅不可以到场的节点深度为$j$的方案数，但是会发现，一个子树内的节点可以被子树外的节点覆盖，而不被子树内的节点覆盖

注意到，如果一个子树内的节点被子树外的覆盖了，那么子树内不可以到场的节点一定不比子树外的优，那么就不需要记录最浅的，改为记录最深的没有被覆盖的节点

设$dp(i,j,0/1)$，$0$表示子树内没有未被覆盖的节点，$j$为最浅的不可以到场节点的深度，$1$表示子树内有未被覆盖的节点，$j$为最深的未被覆盖节点深度

# CF578F Mirror Box

因为题目要求所有盒子中的边都要被恰好一条光线穿过，那么不妨先将每一条边的中点看作一个端点，那么在一个格子里面就有4个点，在格子里面放一块镜子就是将斜对着的两对顶点连接起来，最后要求相邻两个节点是在同一个联通块内，不能有环，总共有$n+m$​个联通块

但是这样做会发现有两条边是互相关联的，那么很难处理，那么考虑将两条边平移一下缩成一条边，那么就将一个格子4个点看作顶点，那么总共有$(n+1)*(m+1)$个顶点，一个格子内连边的方向就是镜子的方向

![QQ图片20210819103059](D:\Blog\image\QQ图片20210819103059.jpg)

然后可以将这些点黑白染色，会发现边一定是连接了两个相同颜色的顶点，考虑这个图满足什么条件

首先这个图不能有环，那么构成的一定是一个森林

由于外围的轮廓上相邻两个点要联通的方案有两种，那么光线必须封闭在两个点之间，可以发现两个点一定是同色的，那么就可以知道，一定有一个颜色的所有顶点形成了一棵树，这样才能满足封闭的条件，那么剩下没有填入镜子的格子，就是连接另外一种颜色的顶点，在确定树之后，这种方案只有一种

那么只需要一开始用并查集缩点，然后利用矩阵树定理求得两个种方案的方案数，答案就是之和

# CF1558D Top-Notch Insertions

## 题目大意

考虑插入排序的过程，给定$m$个二元组$(x,y)$表示将排序过程中将$x$​位置插入到$y$的位置上

求出有多少个不同的序列满足$m$个二元组
$$
m\leq n\leq 2*10^5
$$
复杂度要求$m\log n$

## 算法讨论

那么首先考虑这个$n$个位置上的数在最终排好序的序列中会排在哪些位置，将其记作$p$

由于排好序的数列是递增的并且可能存在元素相同，那么就相当于将$p$分成若干段，每一段要求是相同的数

由于插入的位置是相同数的最后一个位置，那么要求$p$在同一段的元素应该是递增的

那么考虑$p$​的一个极长连续上升序列，长度为$len$，其关于段数的生成函数为
$$
x\sum\limits_{i=1}^{len-1}\binom{len-1}{i}x^i=x(x+1)^{len-1}
$$
假设有$k$段极长上升序列

那么总的生成函数就是
$$
x^k(1+x)^{n-k}
$$
得到答案就是
$$
\begin{align}
&\sum\limits_{i=k}^{n}\binom{n-k}{i-k}\binom{n}{i}\\
&=\sum\limits_{i=k}^n\binom{n-k}{n-i}\binom{n}{i}\\
&=\binom{2n-k}{k}
\end{align}
$$
那么只需要找出$k$就可以了

那么直接模拟这个过程可以用平衡树来实现，就是将没有出现过的$x$分成一段一段，然后看作一个点插入到平衡树中，但是这样实现起来实在比较麻烦

正着考虑不行，那么就倒着考虑

那么我们从后往前确定每一个数的位置，那么相当于一开始所有位置上都有数，查出第$y$个位置，那么这就是$x$对应的$p$，确定完之后把这个位置上数删除

这个过程可以用树状数组实现（只要写一个树状数组上二分求第$k$大就可以了）

考虑k也是等于相邻两个下降次数+1的，那么在确定一个位置之后，由于是往前面插入的，那么只需要检查一下确定位置之后的那个位置是否还没有删除，如果没有删除一定是比当前x小的

# CF375C Circling Round Treasures

## 题目大意

给定$n\times m$的网格，网格上有障碍，炸弹，宝藏（保证炸弹和宝藏的数量之和$\leq 8$），每一个宝藏都一个权值，现在要走出若干条封闭的路径，使得宝藏被包含在路径之内，并且炸弹不能被包含在内

一个方案的获益为$\sum v_i-step$，$step$为走过的步数
$$
n,m\leq 20
$$

## 算法讨论

还是需要使用射线法，就是判定一个点在路径之内的依据

设$dp(i,j,mask)$表示当前从起点走到$(i,j)$，mask表示所有物体发出的射线经过了奇数次还是偶数次

我们假定所有物品引出的射线都是向右的，并且偏离了一个小的角度

那么如何更新一个答案

用bfs来进行转移即可，有两种情况会改变当前的交点数量，记当前点为$(x,y)$，走到的点为$(x+dx,y+dy)$，引出射线点的坐标为$(nx,ny)$

- 如果$nx=x,y\geq ny,dx=-1$，那么增加一个交点
- 如果$nx=x+dx,y+dy\geq ny,dx=1$，那么增加一个交点

由于dp更新出来就是默认可以重复走多次，$dp(s_x,s_y,mask)$就是走过包含mask的走过的最小步数

那么就不需要$O(3^n)$枚举子集了，直接统计答案就可以了

# CF1290F Making Shapes

## 题目大意

![2021-09-10 15-34-51屏幕截图](D:\Blog\image\2021-09-10 15-34-51屏幕截图.png)

## 算法讨论

首先一个选择向量的合法方式唯一对应着一个凸多边形，如果对于第$i$个向量选择了$p_i$个

那么有如下条件成立

- $\sum\limits_{x_i>0} p_ix_i=-\sum\limits_{x_i<0}p_ix_i=X\leq m$
- $\sum\limits_{y_i>0} p_iy_i=-\sum\limits_{y_i<0}p_iy_i=Y\leq m$

那么要求的是有多少个$(p_1,p_2,...,p_n)$满足这个条件

一开始的想法：觉得这是个不定方程，要求解的组数并且和的还有限制，想着用两个变量表示出其他变量，但是无法做的，后来有想了一些在代数上变形的做法，都没有跳出这个常规的圈子

这个是常规方法处理不了的问题，其原因常规方法绕不过枚举的问题，如果直接枚举会有很多选择空间，**说白了就是一次考虑的太多了，可能就需要将若干次的枚举组合起来**

考虑二进制从低位到高位一位一位的进行考虑，假设用$a,b,c,d$替代$\sum\limits_{x_i>0} p_ix_i,-\sum\limits_{x_i<0}p_ix_i,\sum\limits_{y_i>0} p_iy_i,-\sum\limits_{y_i<0}p_iy_i$，如果我们记录之前进位的内容，前面我们只需要知道已经枚举过的位是否比$m$大两个信息

那么就可以用DP了，设$dp(i,a,b,c,d,tx,ty)$表示考虑到第$i$位，进位情况为$a,b,c,d$，其中$x$维的和前$i$位是否大于$m$，$y$维的和前$i$位是否大于$m$的方案数

那么转移的时候，枚举$n$个$p$当前这一位是否为$1$，处理进位情况，然后$a,b$的当前位要相同，$c,d$的当前位要相同，然后根据当前位和m当前位的情况更新$tx,ty$

其中进位的范围为$4n$

那么时间复杂度$O(2^n n^4 \log m)$，常数有1024之大

# CF1574F Occurrences

## 题目大意

给定$n$个序列$A_1,A_2,...,A_n$，其中元素都是$\in[1,k]$，现在要构造一个长度为$m$的序列$a$，使得$A_i$在$a$中出现的次数不少于$A_i$任意一个非空子串在$a$中出现的次数

求构造$a$的方案数
$$
n,m,k\leq 3*10^5,\sum|A_i|\leq 3*10^5 
$$

## 算法讨论

首先可以发现，只要$A_i$中每一个元素在$a$中出现的次数不少于$A_i$本身在$a$中出现的次数，是一定合法的

如果某一个$A_i$中存在两个相同的元素，那么$A_i$中所有元素都是不能出现在$a$中的

考虑两个序列$A_i,A_j$

- 如果为子串关系，那么长度较小的那个序列是不能单独出现在$a$中的，那么只需要保留长度比较大的那个序列即可

- 如果不为子串关系，如果字符集不相交，那么都是需要保留的

- 如果不为子串关系，并且字符集相交，那么相交的那部分字符集一定是一个在前缀一个在后缀，使得这两个串可以拼接成一个新的串并且不存在一个字符出现过两次
- 如果不为子串关系，并且字符集相交，不满足上面的条件，那么$A_i,A_j$中所有元素都不能出现在$a$中

经过上面的处理之后，得到的序列之间字符集都是不相交的，并且一个序列中不会出现两个相同元素，那么$a$就是由这些串拼起来的

设这些串的长度为$g_1,g_2,...,g_{n'}$，考虑设$dp(i)$表示当前长度为$i$的方案数
$$
dp(i)=\sum\limits_{i=1}^{n'} dp(i-g_i)
$$
可以直接分治fft，还有就是可以由于$\sum\limits_{i=1}^{n'}g_i=k$，那么不同的$g_i$只有$\sqrt k$种，那么将相同的$g_i$放在一起转移，时间复杂度就是$O(m\sqrt k)$

考虑如何求出一个序列是否要保留，一种方法是直接进行模拟，但细节很多，容易出错，一种简单的方法是，考虑将$A_{i,j}$向$A_{i,j+1}$连一条边

可以发现的是如果一个联通块不为链的话，一定都是不合法的，对于是链的联通块来说，就是最终保留拼接得到的串

# CF1530H Turing's Award

首先我们可以考虑将整个过程倒过来进行考虑，那么这样的话所有格子的数字就是走过的第一个数字，那么显然走过的位置形成了一段区间，并且我们并不需要关系其在数轴上的绝对位置，我们只关心各个元素之间的相对关系

下面是一个关键的想法，就是考虑的DP过程关于LIS的信息是无法用$O(1)$个变量进行表示的，并且对我们的移动操作来说，由于可以停留在原地，那么如果向某一个方向拓展一格我们可以选择任意的比当前填的数小的数进行操作，那么我们可以考虑去除那些不在LIS中的数字，相当于我们最终得到的序列就是得到的LIS

事实上我们是可以做到的，除了一种情况就是由于我们需要强制选择$a_n$放到序列中如果LIS中不存在$a_n$这个元素的话，那么实际的序列长度需要+1

然后还需要用到一个结论就是，**对于一个任意的排列来说，其LIS和LDS的长度期望是$O(\sqrt n)$**，由于我们形成的序列可以从一个IS和一个DS组成，那么显然长度也是$O(\sqrt n)$的

那么我们就可以考虑DP了，由于我们只关心可能在LIS中的序列，那么我们只需要记录这个序列的开头元素和结尾元素还有这个序列的长度即可，并且假设当前使用到$i$这个数，强制当前位置在两个端点其中一个，那么说明开头元素和结尾元素其中之一为$i$

那么设$f_L(i,len)$表示当前i在左端点LIS的长度为$len$的最小可能右端点，$f_R(i,len)$的定义类似

那么就有转移
$$
f_L(i,len)=\min \limits_{j>i,a_j>a_i} f_L(j,len-1)\\
f_L(i,len)=\min\limits_{j\geq i+len-1,f_R(j,len-1)>a_i} a_j
$$
对于$f_R(i,len)$的转移是类似的，那么我们可以通过用树状数组来优化这个转移

由于状态总数为$O(n\sqrt n)$

那么总复杂度为$O(n\sqrt n\log n)$

# 
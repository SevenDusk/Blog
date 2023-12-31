# 8.20

## D.All Kill

### 题目大意

在一场比赛中有$n$个问题，时长为$t$，每一个问题都有一个写代码的时间$x_i$，每一个问题都会在比赛的某一个时刻想出来，并且优先选择下标较小的问题写代码，注意可以中途换题

那么现在要求所有问题都能在比赛的时候解决，并且所有题目写代码的时间都是连续的方案数
$$
n\leq 10^5
$$

### 算法讨论

考虑最终的分配方案，那么可以将所有问题所占据的连续段缩成一段，然后考虑从小到大展开这些缩起来的段

这样的连续段有$t-\sum\limits_{i}(x_i-1)$个，然后加入第一个的方案数就是$t-\sum\limits_{i}(x_i-1)$​，然后加入第二个的时候，需要注意的是可以将想出来的时间点放在写第一个问题的时间段里面，那么方案数就要加上$x_1-1$

但是这样做会发现，如果当前问题$i$想出来的时间是加入写其他的问题$j$的代码的时间里面，然后写完$j$之后如果没有空余的时间段的话，问题$i$就不能在规定的时间内写出来，就是不合法情况，没法处理这种情况

**那么就需要将所有时间段看作一个环**，这样就不会存在没有空闲时间段的情况，由于我们需要将环断开，那么需要加入一个空闲的段，然后结束上面的分配过程后，找到一个空闲段，从那里断开

那么这样最初就有$k=t+1-\sum\limits_{i}(x_i-1)$个段，那么相应的方案数都要+1

由于每一种方案都被统计了$k$​次（就是任意旋转），那么总方案数除以$k$即可

几个想不清楚的问题：

1.为什么加入新的空闲段不会对总方案数造成影响？这是因为最后我们把重复统计的除掉了，相当于把这个影响消除掉了

2.为什么会想到看作环？这是因为如果看作一个序列的话，因为存在特殊的位置（首尾），会有一些情况没法考虑，但是环每一个位置都是相同的，不会存在特殊位置需要特殊考虑的情况，并且这个性质，即使有一些方案会被算重，但被算重的次数是固定的

# [8.21](https://codeforces.com/gym/101667)

## A.Broadcast Stations

### 题目大意

给定一棵树$n$个点的树，可以给每一个点设一个权值$p_i$，使得从这个点出发距离小于等于$p_i$的点都是被这个点管辖的，现在要使所有点都被管辖，最小化$\sum\limits_{i=1}^n p_i$
$$
n\leq 5000
$$

### 算法讨论

由于一个子树内要么所有点都被管辖了，就只需要记录这个子树向外能管辖的范围，要么就存在点没有被管辖，就只需要记录离当前子树根节点最远没有被管辖的距离就可以了（因为现在需要管辖子树内的节点一定需要通过根）

那么就启发我们记$f(x,i)$表示当$x$子树内所有节点都被管辖的时候，从$x$节点向外管辖范围最大为$i$的最小花费，设$g(x,i)$表示当前$x$子树内存在节点没有被管辖，并且最深的那个节点到$x$的距离为$i$的最小花费

那么有如下六种转移
$$
f(x,i)+f(u,j)\rightarrow f'(x,\max(i,j-1))\\
f(x,i)+g(u,j)\rightarrow_{i\geq j+1} f'(x,i)\\
f(x,i)+g(u,j)\rightarrow_{i<j+1} g'(x,j+1)\\
g(x,i)+f(u,,j)\rightarrow_{i\leq j-1} f'(x,j-1)\\
g(x,i)+f(u,j)\rightarrow_{i>j-1} g'(x,i)\\
g(x,i)+g(u,j)\rightarrow g'(x,\max(i,j+1))
$$
其中前5种事实上都是一种max卷积的形式，不能直接转移，直接转移的复杂度是$O(n^3)$，那么只要讨论一下max取到的是$i$还是$j$，同时记录一下对应的前缀MAX，就可以做到$O(1)$转移，那么复杂度就是$O(n^2)$

至于最后一种，不需要那么麻烦，直接转移就行了，因为这个转移是严格小于树形背包的复杂度的

总复杂度$O(n^2)$

## J.Strongly Matchable

### 题目大意

给定$n$个点$m$条边的无向图，问对于所有将顶点划分成两个不相交集合，只考虑两个集合之间的边，问是否所有情况都存在完美匹配
$$
n\leq 100
$$

### 算法讨论

考虑对于划分出来的二分图用Hall定理，考虑一个集合$S,|S|\leq\frac{n}{2}$，将其$S$中所有点连出的点去除$S$中点所形成的集合$N(S)$​，最坏情况下$S$所在二分图的一边剩下的节点都从$N(S)$中选出来，那么根据Hall定理有
$$
|N(S)|\geq |S|+\frac{n}{2}-|S|=\frac{n}{2}
$$
那么我们就得到了这个图是满足条件的充分必要条件：对于任意集合$S$，相邻节点所组成集合$N(S)$的大小要$\geq \frac{n}{2}$

那么我们就需要寻找最小的$|N(S)|$

首先可以发现的是，**如果$S$​​​不连通，一定不优**，因为选取其中一个联通块都是比这个整体优秀的

那么我们就考虑向$S$​中加入新点的过程，如果记加入点的邻接点集合为$e(i)$​

- 如果$e(i)\subset S\cup N(S)$​的话，加入这个节点，就相当于从$N(S)$​中删去$i$​，然后在$S$​加入$i$​，就可以将$|N(S)|$​减1

- 如果不满足的话，就一定会将$N(S)$​的大小增加，显然不会这样选

考虑最开始的时候是只有一个点的，由于不存在不满足条件$e(i)\subset S\cup N(S)$的选择，那么相当于确定了起始点，就确定了最优的集合$S$

那么考虑枚举起始点$i$，选出**与$i$相邻**并且满足条件的所有点（**需要注意的是由于$|S|\leq \frac{n}{2}$，那么最多只能选出$\frac{n}{2}-1$个点**），如果算出的$N(S)<\frac{n}{2}$​，就判定这个图不合法

时间复杂度就是$O(\frac{n^3}{w})$

# [8.24](https://codeforces.com/gym/102483)

## F.Fastest Speedrun

### 题目大意

现在有$n$个关卡，在打败第$i$个关卡的之后，可以获得一把等级为$i$​的武器，并且不同的武器打败第$i$的关卡的时间也是不同的，$a_{i,j}$表示等级为$j$的武器打通第$i$个关卡所需要的时间，并且等级高的武器一定比等级低的武器通关时间短或者一样，并且每一个关卡有对应的特殊武器$x_i$，用这个武器打通关卡的时间为$s_i$，满足$s_i\leq a_{i,j},\forall j\in[0,n]$

最初有一把武器等级为$0$，可以任意顺序选择关卡，求出最小花费时间
$$
n\leq 2500
$$

### 算法讨论

首先让所有关卡的特殊武器向这个关卡连一条边就是$x_i\rightarrow i$

显然连出来的图是一个基环外向树，由于选择特殊武器一定最优，那么如果得到第$i$​级武器，那么可以将子树内所有的关卡通关，其实基环树的环是可以缩成一个点的，对应的$a$取min就可以了

那么相当于当前有一个武器用来解锁树上的关卡，然后通过解锁的关卡来获得更高级别的武器

显然一开始就不断地升级武器，然后解锁第$n$级的武器，用这个$n$级武器解锁在升级过程中没有解锁的关卡

那么首先将树上每一个节点的子树内级别最大值求出来，记作$MAX_i$，表示解锁这个关卡可以得到等级为$MAX_i$的武器，那么可以发现如果我们要得到n级武器，那么一定是要解锁一个树的根最优

并且那些中途没有解锁的关卡一定是选在一个树的根上，只要用$n$​级武器解锁这个树的根就可以了

我们令$a'_{i,j}=a_{i,j}-s_i$，那么我们只需要关注那些没有用特殊武器的关卡即可

然后我们从后往前考虑这个过程，用一个DP进行统计即可，设$f(i)$表示当前为武器级别为$i$​的最小代价

转移的时候枚举上一次的武器，和当前$MAX_x=i$的节点，求出代价即可

需要注意的是可能存在$x_i=0$的情况，那么起初的武器就不是0级，就是其中最大级别

## D.Date Pickup

### 题目大意

给定$n$个点$m$条边的有向图，保证每一个节点都有出边，走过一条边需要一定的时间，现在从$1$号节点出发，定义一条路径的权值为在时间$[a,b]$中路径上最大到节点$n$​​最短路长度，求出最小权值

注意可以最初在1号节点停留，但在其他节点不能停留
$$
n,m\leq 10^5,a,b\leq 10^{12}
$$

### 算法讨论

首先肯定是二分答案$mid$，那么剩下判断$mid$是否合法

一开始的想法是把所有可行的点和边（就是如果在这个点或边上收到了信号，那么就可以在mid时间走到n号节点）直接找出来，然后问题就变成在找出来的这些点中是否可以停留b的时间

**但是很难找到一个简单的判断条件，就需要考虑先找出一些比较显然的可行节点，然后通过这些节点扩展出其他可以节点**

首先一些简单的情况，记$d(u,v)$表示从$u$到$v$的最短路

首先如果一个节点$x$满足$d(1,x)+d(x,n)\leq a+mid$​，那么只要在家里等待适当的时间出发，沿着最短路走一定合法

然后考虑从这些节点扩展出其他合法的节点

如果存在$u\rightarrow_l v$这样一条边，并且$u$是合法节点，满足$l+d(v,n)\leq mid$，那么就可以将这条边和$v$​设为合法

依次不断更新下去（可以用bfs实现）就可以得到原图的一个子图，显然1号节点和n号节点一定会判断为合法，那么我们就只需要判断留在这个子图中的时间是否可以$\geq b$

首先可以在1号节点待$a+mid-d(1,n)$的时间，然后就需要走出1号节点

如果这个子图有环，那么就可以走到这个环上不断走，就一定合法

那么我们只需要考虑DAG的情况，这个只需要求一个最长路就可以了

**难点就是在如何求出合法的点集，并且不要忽略了可以在1号节点待一段时间的这个条件**

# [8.25](https://codeforces.com/gym/102001)

## B.Rotating Gear

### 题目大意

给定一个n个节点的树，每一个节点都是一个齿轮，树上的边表示两个齿轮相接，顺时针旋转一个齿轮$\alpha$度，所有与它相邻的齿轮都会逆时针旋转$\alpha$度，每一个齿轮上都有一个指针，最初指向正上方

现在有三个操作，第一个是将一个齿轮从平面上卸下，第二个是将一个齿轮放回并保证指针的方向不变，第三个是将一个齿轮顺时针旋转$\alpha$​度

对于每一个第三个操作，需要求出旋转的齿轮数量乘上旋转的角度

经过所有操作，求出所有齿轮指针偏转角度之和
$$
n\leq 10^5
$$


### 算法讨论

首先考虑对于每一个第三个操作如何回答

**那么问题就是需要动态支持删点加点和联通块大小，那么可以将每一个联通块中深度最小的那个节点设为这个联通块的代表节点$rep$**，如果设$v_x$表示$x$​子树内与$x$联通的节点数量，那么一个联通块的大小就是$v_{rep}$，对于联通块中一个节点求出rep也是很好找的，只要找到这个点到根节点最浅的被删除的节点下面那个就是

接下来考虑删点加点如何动态维护$v$，这时候需要拓展一下$v_x$的定义，如果$x$是被删除的节点，那么$v_x$表示的就是如果加入这个节点的话，子树内与x联通的个数

那么删除一个节点$x$的时候，记$y$表示是x到根路径上第一个被删除的节点，那么只需要将$fa_x\rightarrow y$的路径上$v$减去$v_x$即可，加入节点的时候也是同理

那么现在需要考虑所有齿轮指针偏转角度之和，由于这个是在所有修改操作之后询问1次

注意到对于把节点$x$旋转$\alpha $度，与$x$距离为奇数的节点一定是旋转$-\alpha$度，偶数的节点旋转$\alpha $度，那么我们可以对于每一个节点都记录一个二元组$(a,b)$表示如果当前节点的深度为奇数，那么旋转$a$度，否则旋转$b$度

那么相当于需要给当前联通块内所有节点加上一个二元组

那么就需要考虑一下容斥计算这个贡献，我们在给一个联通块加上二元组的时候，给这个联通块的rep子树内都加上这个二元组，显然这样会有一些不是属于这个联通块的节点被算到

但是我们注意到那些被删除的节点上也会记录这个贡献，那么如果在这个节点加入的时候，将其下的所有节点去除这个贡献，那么也就可以了

需要注意可能存在多层嵌套的关系，一层$x$内部的加入之后需要考虑上一层$y$，子树内减去的是$w_x-w_y$

删除的时候就需要清空当前的二元组，加入的也是$w_x-w_y$

那么只需要树链剖分和BIT维护就可以了

## C.Smart Thief

### 题目大意

给定大小为$m$的字符集，构造一个长度最小的字符串，使得里面至少出现了$k$个本质不同的长度为$n$的子串
$$
n\leq 10^5,m\leq 10,k\leq \min(m^n,10^5)
$$

### 算法讨论

猜想最短的字符串长度为$n+k-1$

最初的想法是对所有长度为$n$的串建图，构造哈密顿路径，然后感觉如果$m^n$比较大的时候还可以随机

事实证明前面一个想法明显是无法实现的，**哈密顿路径本来就是一个NPC问题，既然不能当作点，那么可以考虑转化为边，变成欧拉路径，那么就好解决多了**

那么就需要考虑所有长度为$n-1$的串，考虑之间的转移，如果$S$可以通过删去第一个字符，最后加入一个字符得到$T$，那么就在$S$和$T$之间连一条边

走过一条边相当于就是一个长度为$n$的字符串，只要不重复经过这条边就可以了

这张图规模可能很大，那么就不能显式的建出图，只能在求欧拉路径的过程中不断动态建出图来

首先需要如果dfs过程中深度已经等于$k$了，那么直接输出在dfs路径上的边（这点很重要，因为求欧拉路径的过程中是在dfs完之后才加入边的，可能加入第一条边的时候已经dfs很深了）

然后就是考虑加入欧拉路径的边数如果等于$k$就直接输出

但是这样在$n\geq 50$的时候速度很慢，注意还可以随机由于$n\geq 40$的时候$m^n\geq 2^n$已经很大了，如果随机$n+k-1$位冲突的概率很小，那么直接随机输出一个字符串就可以了


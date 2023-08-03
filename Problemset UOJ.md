# #22. 【UR #1】外星人

一开始随便搞出第一问答案，很显然的性质对$x$有变化的$a$一定是递减的，就拿一个桶直接记录可以达到的值

然后我开始想第二问，一开始想直接在这个桶上统计答案，然后发现不行，之后再想，如果利用上面的性质，在选取了一个$a_i \leq x$时，会有一段区间的$a$可以随便插入到$a_i$之后，然后就被一些组合数学的细节绕晕，没有想清楚，这一段区间是$(x \mod a_i,x]$，并且要在$a_i$中挑一个出来放在最前面，然后会发现$x \mod a_i$是一个子问题，搞出来这个合法方案排列数后，将$(x \mod a_i,x]$中的数，插入排列中，方案数$(sum[x\ mod\ a_i]+1)(sum[x\ mod\ a_i]+2)...(sum[x]-1)$，即$\frac{sum[x-1]!}{sum[x\ mod\ a_i]!}$

那么就设$dp[i]$为当前$x=i$时最大答案，$f[i]$为$x=i$且满足最大答案时的方案数

$dp[i]=\max dp[i \mod a_j]$ $(a_j \leq i)$

然后f[i]由那些可以得到最大答案的$f[i \mod a_j]$根据上面的更新得到 

[代码](https://uoj.ac/submission/435845)

# #33. 【UR #2】树上GCD

首先考虑枚举$LCA$，那么就是统计统计利用不同子树各个深度的信息来统计答案，由于是深度信息，那么考虑利用长链剖分来维护这个信息，首先常规套路是继承重儿子信息，暴力维护轻儿子信息

先假设有两个数组$a,b$其中$a[i],b[i]$分别表示以该选定的$LCA$为根时，某一棵子树内深度为$i$的点的个数

考虑枚举答案$k$，那么开始推式子

$\sum_{i=1}^{n} \sum_{j=1}^{m} [gcd(i,j)=k]a[i]*b[j]$

$\sum_{i=1}^{\lfloor \frac{n}{k} \rfloor} \sum_{j=1}^{\lfloor \frac{m}{k} \rfloor} [gcd(i,j)=1]a[ik]b[jk]$

$\sum_{i=1}^{\lfloor \frac{n}{k} \rfloor} \sum_{j=1}^{\lfloor \frac{m}{k} \rfloor} a[ik]b[jk] \sum_{d|gcd(i,j)} \mu (d)$

$\sum_{d=1}^{\lfloor \frac{n}{k} \rfloor} \mu(d) \sum_{i=1}^{\lfloor \frac{n}{kd} \rfloor} \sum_{j=1}^{\lfloor \frac{m}{kd} \rfloor} a[ikd]b[jkd]$

$\sum_{d=1}^{\lfloor \frac{n}{k} \rfloor} \mu(d) (\sum_{i=1}^{\lfloor \frac{n}{kd} \rfloor} a[ikd]) (\sum_{j=1}^{\lfloor \frac{m}{kd} \rfloor} b[jkd])$

在维护答案的时候，会产生对于一个数组下标为kd倍数的个数和，并且可以发现$a,b$数组可以简单的维护，只是每次重儿子继承上来的信息不能每一次都遍历一遍（轻儿子可以）

那么考虑根号分治

对于$> \sqrt n$的询问直接暴力查询，每一次查询是$\sqrt n$的时间复杂度，最终均摊时间复杂度为$O(n\sqrt n)$

对于$< \sqrt n$的询问，这样进行考虑，由于每一次dfs下去之后，进行重儿子继承的是一段重链不断向上更新，那么考虑记$dp[i][j]$表示当前重链继承上来深度$%i=j$的个数有多少，每次询问时$O(1)$，轻儿子合并时，更新$dp$值就可以了，然后如果更新到重链顶时，就清空$dp$数组（注意不能直接把所有的dp值都清空，只能清空用过的$dp$值，否则时间复杂度不对）那么均摊复杂度也为$O(n\sqrt n)$

[代码](https://uoj.ac/submission/436217)

# #32. 【UR #2】跳蚤公路

首先题目的意思就是让我们找出负环，解出$x$在环上不等式的解集

然后可以发现一个环的解集一定是一个形如$kx+b \geq 0$的形式，那么对于某一个点，其解集一定是一个连续的区间$[L,R]$，这样可以二分出来，注意这里先需要找出合法区间内的一个合法解，之后再能两次二分出左右端点，求一个合法解的方法也是二分，二分出$mid$后，如果不存在负环，那么说明这一定是一个合法解直接返回即可，如果有负环，那么用spfa跑出来之后，看是$+1$还是$-1$的边使得当前的图存在负环，只要记录一下到每一个点的$+1/-1$之和，如果为正，那么需要变大，如果为负，那么需要变小

那么最暴力的方法对于每一个点直接二分出这个区间，这样复杂度为$O(n^2mlogw)$

考虑强连通分量缩点，那么负环只有可能存在于强连通分量中，缩点后的图是一个DAG，最终某一个点的答案一定是1号点到这个点在DAG上走过所有强连通分量解集的交

那么有效的环一定是在某一个强连通分量中，只要对每一个强连通分量二分求出其合法区间即可，那么这个算法的上界是$O(nmlogw)$

那么最后合并答案的时候，只要进行拓扑序，然后上界取$min$，下界取$max$即可

题解中给出另外一种做法

考虑在spfa结束后，什么样的节点是在负环上的

设$f(n,i)$表示经过$n$轮之后到达$i$的最小路径

对于节点$i$，$i$在负环上的充要条件是

$f(n,i) \leq f(n-1,i)$

现在设$g(n,i,k)$表示经过$n$轮之和到达$i$并且到到这个节点的不等式$x$前的前的系数为$k$最短路径

$f(n,i)=\min\{g(n,i,k)+kx\}$

带入到上式得$ \min\{g(n,i,k)+kx\} \leq \min\{g(n-1,i,j)+jx\}$

解这个不等式即可

代码我只写了我的那个做法

[代码](https://uoj.ac/submission/437921)

# #310. 【UNR #2】黎明前的巧克力

感觉挺妙的，窝还是太菜

基本上可以一眼看出来这是XOR卷积的形式，每一个数可以放入两个集合中的一个或者不放，只要两个集合中的所有数异或起来等于$0$，那么这个方案就是合法的，但是由于集合不能相交，那就不能简单地把所有数放到对应位置上，然后直接自乘

那么需要一个个数的拆开分别卷起来，那么对于某一个数$a_i$，其对应的多项式$f(x)[a_i]=2,f(x)[0]=1$，需要把$n$个这样的多项式XOR卷积起来，得到的最终多项式才是最终答案

考虑在对一个多项式做FWT的时候得到的FWT每一位具体是什么

$FWT(A)[i]=\sum_{cnt(i\&j)=0} A_j-\sum_{cnt(i\&j)=1} A_j$

$0$位的对于每一位的贡献都是$+1$，而$a_i$对每一位的贡献要么是$+2$要么是$-2$，所以最终FWT出来的每一位要么是$3$要么是$-1$

那么如果计算出在所有多项式中每一位出现过多少个$3$，出现过多少个$-1$，就可以通过计算快速幂得到最终多项式

由于FWT是线性变换，那么$FWT(A+B)=FWT(A)+FWT(B)$，那么考虑将所有多项式相加压缩得到一个多项式，做一遍FWT，那么得到某一位上的值就是所有多项式FWT之后结果这一位值之和

设这一位上有$x$个$3$,$y$个$-1$

那么$3x-y=FWT[i],x+y=n$

解得$x=\frac{n+FWT[i]}{4},y=n-\frac{n+FWT[i]}{4}$

那么直接快速幂，IFWT回去即可

[代码](https://uoj.ac/submission/438343)

# #61. 【UR #5】怎样更有力气

首先可以想到可以将所有的加边操作按$w$排序，然后依次加入边

考虑如果$p=0$的时候，相当于现在没有任何限制，那么把所有$u$到$v$路径上所有点连成一个联通块，具体实现可以用一个并查集，维护，在把所有路径上节点找出来的时候，每一次跳的时候找到并查集中的根，然后跳过去，最后把这个路径缩成一个点

但如果有限制这样做是不行的

可以发现如果每一个操作的限制个数小于$u$到$v$的路径长度，那么一定可以把这个路径连成一个联通块，套用上面的做法即可

但如果限制个数大于等于路径长度，那么就有可能不能把整个路径连成一个联通块，那么现在的目标就是尽可能地去合并点

首先如果两个点没有限制，那么可以合并整个两个点，但之前枚举$w^2$（$w$为这个路径上的点数），复杂度是错的

那么考虑去找到一个中继节点$rt$，使得尽可能多的点不跟其有限制，可以发现这个节点连出的限制最多只有$\sqrt(w)$个

设$S$为$rt$限制到的集合，$T$为$rt$没有限制到的集合

如果对于$x\in S$，$\exists y\in T$，$x$没有到$y$的限制，那么可以把$x$合并到$rt$上

然后暴力枚举两个$S$中的元素，判断能否合并，这一部分复杂度$O(w)$

总复杂度$O((p+n)logn)$

还有注意上面两钟情况，要分别维护两个并查集

[代码](https://uoj.ac/submission/441257)

 

# #50. 【UR #3】链式反应

题目可以转化为求$n$个节点可以产生二叉树，并且树上每一个节点可以外挂节点（也就是题目中的破坏死光照射到的节点）的数量，并且这个二叉树标号需要满足堆的性质

那么可以简单的设$dp[i]$表示用$i$个节点产生的二叉树的数量有多少个，其中$dp[0]=0$，$a[1...m]$表示可以外挂的节点数量（包含当前的根节点，也就是$01$串为$1$的下标$+1$）

那么$dp[n]=\frac{1}{2}\sum\limits_{i=1}^{m}\sum\limits_{j=0}^{n-a_i}\binom{n-1}{a_i-1}\binom{n-a_i}{j}dp[j]dp[n-a_i-j]$

直接做可以得到$40$分

考虑优化，继续推式子

$dp[n]=\frac{1}{2}\sum\limits_{i=1}^{m}\sum\limits_{j=0}^{n-a_i}\binom{n-1}{a_i-1,j,n-a_i-j}dp[j]dp[n-a_i-j]$

$\frac{dp[n]}{(n-1)!}=\frac{1}{2}\sum\limits_{i=1}^{m}\frac{1}{(a_i-1)!}\sum\limits_{j=0}^{n-a_i}\frac{dp[j]}{j!}\frac{dp[n-a_i-j]}{(n-a_i-j)!}$

推到这一步，如果把$a$看作一个在值域上的多项式$s$，那么这个就可以直接用分治$FFT$做了，分治的过程中维护一下$dp$和自己的卷积，$dp$和$s$的卷积，就可以计算得到所有的$dp$值，时间复杂度$O(nlog^2n)$，要注意的是维护$dp$与自己的卷积的时候，需要注意计算左边对右边贡献，那些用之前计算的$dp$值来卷$[l,mid]$的时候，如果次数$<l$，那么之前的系数要设为$2$，因为在做卷积的时候，这两个数相乘是做$2$次的

然后再考虑生成函数，设$F(x)=\sum\limits_n \frac{dp[n]}{n!}$

那么$F'=\frac{1}{2}s(x)F^2+1$

直接解是解不出来的

那么套用解一阶微分方程的牛顿迭代做法就可以了，时间复杂度$O(nlogn)$，巨大常数，根本跑不过分治$FFT$

[代码](https://uoj.ac/submission/442360)

# #36. 【清华集训2014】玛里苟斯

首先需要发现的是，只有这些数的基有用，这是应为如果存在某一个数$a_i$，使得存在不包含$a_i$的一个集合$S$，其异或和等于$a_i$

那么对于$S$的任意一个子集$T$所得到的异或和，都可以通过取$T$在$S$中补集，在异或$a_i$得到一个相同的值，那么每一个值就有两种方案，由于求期望，计算的时候会把这个因子$2$约掉，那么去掉$a_i$不影响答案

由于题目保证了答案不大于$2^{63}$，$a$中数的上界为$2^{\frac{63}{k}}$，可以发现如果$k\geq 3$，线性基中元素的个数最多只有$21$个，那么直接暴力统计答案，并且这道题有一个性质是，答案要么是整数，要么是$.5$，这个可以从下面的证明看出来

那么现在只要考虑$k=1,k=2$的情况

当$k=1$时，可以单独考虑每一位上的贡献，对于第$i$位上存在$m(m\neq 0)$个$1$，其期望贡献为$\frac{2^m\sum\limits_{i=0}^{m}[i\%2=1]\binom{m}{i}}{2^m}$，等于$2^{m-1}$

当$k=2$时，由于是乘积，那么只要考虑某两位上乘积的和，对于第$i$位和第$j$位，如果所有数都是$11$或者$00$，那么这就退化成$k=1$的情况，贡献为$2^{i+j-1}$

否则，记$11$的有$a$个，$01$有$b$个，$10$有$c$个，所有不为$00$的个数有$a+b+c$个，那么贡献为

$\frac{\sum\limits_{k=0}^a\binom{a}{k}\sum\limits_{i=0}^b\sum\limits_{j=0}^b[i\%2=k\%2][i\%2=k\%2]\binom{a}{i}\binom{b}{j}}{2^{a+b+c}}$

化简可得$2^{a+b+c-2}$

那么直接暴力即可

[代码](https://uoj.ac/submission/446561)

# #62. 【UR #5】怎样跑得更快

居然做出了UR的C

首先这个解方程显然不能直接去高斯消元，这个矩阵一定有一些性质，再观察这个式子，一脸莫比乌斯反演的样子，再想到一定反演变化实际上就是矩阵互相求逆的过程，说白就是解方程的过程，那么以这个为指导思想，那么来考虑怎么具体做这道题

首先式子里的$lcm(i,j)^d$改写为$\frac{i^dj^d}{gcd(i,j)^d}$，原式化为$i^d\sum\limits_{j=1}^n gcd(i,j)^{c-d}j^d x_j\equiv b_i$

令$b'_i=\frac{b_i}{i^d}$，然后开始大力推式子

$$
\begin{align}
\\&\sum\limits_{g|i}g^{c-d}\sum\limits_{j}^{\left \lfloor \frac{n}{g} \right \rfloor}[gcd(\frac{i}{g},j)=1] (jg)^d x_{jg}
\\&= \sum\limits_{g|i}g^{c-d}\sum\limits_{j}^{\left \lfloor \frac{n}{g} \right \rfloor}(jg)^d x_{jg}\sum\limits_{k|\frac{i}{g}}\mu (k) 
\\&= \sum\limits_{g|i}g^{c-d}\sum\limits_{k|\frac{i}{g}}\mu (k) \sum\limits_{j}^{\left \lfloor \frac{n}{kg} \right \rfloor}(kgj)^d x_{kgj}
\\&= \sum\limits_{t|i} (\sum\limits_{j=1}^{\left \lfloor \frac{n}{t} \right \rfloor}(tj)^dx_{tj})(\sum\limits_{g|t}g^{c-d}\mu(\frac{t}{g}))
\end{align}
$$


令$C_t=(\sum\limits_{g|t}g^{c-d}\mu(\frac{t}{g})),f(t)=\sum\limits_{j=1}^{\left \lfloor \frac{n}{t} \right \rfloor}(tj)^dx_{tj}$

那么莫比乌斯反演可得

$C_if(i)=\sum\limits_{t|i}\mu(t) b_{\frac{i}{t}}$

预处理$C$和$\mu$，那么就可以在$O(nlogn)$时间内得到$f$，然后从后往前解出每一个$x$（注意需要除以$i^d$），最后带回原式判断是否无解

[代码](https://uoj.ac/submission/446622)

# #345. 【清华集训2017】榕树之心

首先考虑判断根是否合法，对于根节点$x$来说，可以使根节点的两个子树内互相抵消，经典结论，当所有子树最大值$MAX$，使得$2MAX\leq (sz[x]-1-sz[son])$时可以全部抵消掉，最终剩下$(sz[x]-1)\%2$，加上自己$x$一个，否则会剩下$2MAX-(sz[x]-1-sz[son])$个点

那么我们就要尽量消掉重儿子子树内的节点，那么可以设dp[x]表示在x子树内最少可以消到$dp[x]$个节点，考虑转移，根据上面的结论，那么只要$2dp[x]\leq (sz[x]-1-sz[son])$就可以进入第一种情况

因为贪心地想，只要可以得到重儿子子树内节点最小值是满足条件的，虽然有可能在取最小值的时候，不一定满足是所有子树的最大值但是可以通过调整来满足条件

那么直接$DP$到根，判断$dp[1]$是否为$1$，如果为$1$那么可行，否则不可行

接下来考虑如何判断其他节点是否可行，可以利用之前记录的信息，从上往下$DP$，记录到当前节点，子树外最少可以消到多少个，按照之前的做法类似换根$DP$即可

[代码](https://uoj.ac/submission/446709)

 

# #346. 【清华集训2017】某位歌姬的故事

首先可以发现有效的数段只有$2q$个，那么可以把每一个限制的左右端点离散化，然后可以得到$2q$个连续的区间，并且可以发现每一个连续的区间可以填的数的上界是相同的，上界就是所有覆盖这一点限制$m$的最小值

考虑如果只有一个限制的时候，方案数可以简单容斥计算，就是$m^{r-l+1}-(m-1)^{r-l+1}$，那么考虑多个限制的时候，就可以容斥地计算，即$\sum\limits_S (-1)^{|S|} f(S)$，其中$f(S)$表示强制满足$S$集合中限制条件的，其他的条件不满足的方案数，记$x\in S$的所有线段的并的长度为$a$，$x \notin S$所有线段的并长度为$b$，那么$f(S)=m^a+(m-1)^b$，那么这个可以用$DP$解决

设$dp[i][j]$表示到第$i$个区间，所选的区间最远到达$j$的容斥计算结果之和，注意需要把$-1$乘入到结果中

转移如下

$dp[i][j]\rightarrow dp[i+1][j]$

$-g(l[i],r[i],j)dp[i][j]\rightarrow dp[i+1][max(j,r[i])]$

$g(l,r,i)$表示$[l,r]$这个区间与长度为$i$前缀扩展的方案数，也就是讨论一下左右端点$l,r$与$i$的大小关系，利用上面计算容斥系数的算法计算即可

注意到任意一个线段只会出现在点的上界为$m$的区域中，那么可以对于上界相同的点单独考虑，算出容斥系数，将所有的容斥系数相乘起来，就是答案

需要特判某一个限制一定不会被满足的情况，并且需要注意常数，要预处理幂次，否则会在extra test被卡

说白了也不是很复杂

[代码](https://uoj.ac/submission/446823)
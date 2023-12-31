最前面：

AT的题都很有思维难度，总结一下一些AT的常规操作

1. 对于有操作的题目，如果正面推不行的话考虑倒推，将操作转化，寻找更好的性质
2. 模型转化，看到某一种的计算的式子，需要考虑有没有更简化的模型可以达到相同的效果
3. 补集转化，正难则反
4. 分析题目性质，计数题要找到一些限制条件或者构造方案使得计数不重不漏



目标：稳定三题

比例：D:0/1 C：3/6 B：2/3

10.12 - 10.22

AGC47:Solved A,B,C,D Aim E

AGC46:Solved A,B,C,D

AGC45:Solved C Aim B

AGC44:Solved A,B,C Aim D

AGC43:Solved A,B,D Aim C

AGC41:Solved B Aim D

AGC40:Solved C

# AGC37C Numbers on a Circle

首先AT常规操作，考虑倒推。那么现在题目就变为

> 1.选择一个下标$i$，将$b_{i}$减去左右两边的数
>
> 2.题目的目标变为从$b$数组变为$a$数组

由于$a,b$都是正整数，在任意一次操作之后，不能使$b$中的数变为负数，那么$b_{i}>b_{i-1}+b_{i+1}$，然后由于$b$中的值只会减小不会增加，所以$b_{i}>a_{i}$

现在考虑如果i满足上述条件，那么$b_{i}>b_{i-1},b_{i}>b_{i+1}$，对于$i-1,i+1$上述条件一定不满足（第一个条件），所以必须先对i进行操作，才能对左右两个下标进行操作，那么其实操作的大体顺序已经定下来了。相当于每三个分一个组，先把满足条件的数放到队列里，对其进行操作之后再判断一下旁边的两个数是否可以进行操作，如果可以就将点加入队列中。

看到官方题解和网上很多题解都是将所有的数都扔到一个堆里每次取出最大的数进行操作，其实并不需要这个堆，因为中间那个数不进行操作的话，左右两边的数一定不会满足条件，其实每一轮操作的顺序都是环上每隔三个数有一个数进行操作，由于每个数互不影响，所以操作顺序不会影响操作数量

复杂度$O(nlogMAX)$

[代码](https://atcoder.jp/contests/agc037/submissions/15695749)

# AGC37B RGB Balls

首先要考虑如何构造出一组最优的合法方案。考虑将每种球分组后以下标排序，然后将相同下标的三种球放在一起进行考虑

$a_1<a_2<a_3<...<a_{n-1}<a_n$

$b_1<b_2<b_3<...<b_{n-1}<b_n$

$c_1<c_2<c_3<...<c_{n-1}<c_n$

也就是将三种球的下标记录在$a$,$b$,$c$三个数组中，最终最优方案的答案就是$\sum_{i=1}^{n} max\{a_i,b_i,c_i\}-min\{a_i,b_i,c_i\}$

记$A$为$max\{a_i,b_i,c_i\}$组成的集合，$C$为$min\{a_i,b_i,c_i\}$，$B$为除$A,C$集合中其他的位置

可以发现$A$中所有元素都是对计算的值产生正的贡献，$C$中所有元素都是对计算的值产生负的贡献，$B$对计算的值无贡献

那么只要使最终的分配方案能使产生贡献的球都是$A,C$集合中的值即可

那么对于一个人，只要满足选到的球先是$C$集合中的，再是$B$集合中的，最后是$A$集合中的

从左往右枚举球，中途记录一下当前该选哪个集合的人有多少个，每次对答案乘上人数即可

[代码](https://atcoder.jp/contests/agc037/submissions/15666596)

# AGC39C Division by Two with Something

首先要将题目转化一下，对于一个二进制数，对其进行操作就是将最后一位取反放到前面，然后删掉最后一位

可以将这个二进制下的数取反复制到原数的前面，那么一个循环中的数可以通过截取所有得到的新数中长度为$n$的子串得到

考虑一个循环是怎么形成的，一个数通过$2n$次操作一定会变回原来的数，相当于前$n$次操作对原来的数进行了取反，后面$n$次操作对已经取反的数再进行一次取反，两次取反就会得到原来的串

再考虑循环在字符串上的意义，一个最短的循环就是拼接起来得到的那个字符串的最小循环节长度

设循环的次数为i，因为通过$2n$次操作一定回到原数，$n$次操作得到取反后的串，那么$i\mid 2n$，$i\nmid n$

循环节内部是有一个串和它的反串拼起来的

枚举$2n$的因子$i$，统计对于存在循环节为i的串的个数，注意是循环节不是最小循环节（最后容斥回去即可）

如果前$i/2$位比$x$的前$i/2$位小，那么之后循环的时候一定在$x$以内

那么只要判断一下前$i/2$位等于$x$的前$i/2$位，然后构造出前$n$个，比较与$x$的大小，如果比$x$小就对统计的个数$+1$即可

但不过此时统计的数量并不是最小循环节的数量，一个循环节会在循环节长度的倍数处重复统计，最后枚举其倍数容斥即可

[代码](https://atcoder.jp/contests/agc039/submissions/15676518)

# AGC36C GP 2

窝连最开始的限制条件都没想到

考虑最终的序列要满足什么条件才能被表示出来。很明显有$\sum_{i=1}^{n} x[i]=3m$，然后考虑一个数最大只能是$m$个$2$加起来，那么$\max_{i} x[i]\leqslant 2m$，由于是一个$+1$另一个$+2$，那么每一次最多改变一个数的奇偶性，那么$\sum_{i=1}^{n} [x[i]\%2=1]\leqslant m$

可以发现操作集合到满足这个限制条件序列的集合是一个满射，这种统计方案不重不漏

只考虑第一个和第三个限制条件，不考虑第二个，那么可以先在序列上若干个$1$，然后再在序列上放$2$，那么可以枚举放1的数量

$\sum_{i=0,(3m-i)\%2=0}^{m}C_{n}^{i}C_{n+\frac{3m-i}{2}-1}^{n-1}$

再考虑第二个限制条件，可以发现如果第二条件不满足，那么只有$1$个数不满足限制，那么考虑补集转化

枚举那个不满足的数有多大，再将剩下的数分配到$n-1$个位置里

再考虑一下第三个限制条件，可以发现，如果一个数大于$2m$，那么序列上不为$0$的数最多有$m$个数，即使全部都为奇数，也符合第三个限制条件，所以不用再考虑第三个限制条件

$n\sum_{i=2m+1}^{3m}C_{n+3m-i-2}^{n-2}$

两式相减即为答案

[代码](https://atcoder.jp/contests/agc039/submissions/15676518)

# AGC34C Tests

可以发现，如果一门考试比另外一个人高了，那么重要度一定要设为$u_i$，并且要学到满分，因为每一门考试都有一个$b_i$，只有超过$b_i$才能反超另外一个人，那么有一门考试已经超过$b_i$，那就要充分利用这门考试，去学其他的考试一定没有比把这个考试学到满分优（因为还要先学到$b_i$才可以）；如果一门考试比另外一个人低，那么重要度一定要设为$l_i$

那么在这种策略之下，最多只有一门考试学习过但没有学到满分，其他的考试要么没有学要么已经学到了满分，原因是可能有边角料的存在，会使得有一门课不是满分

因为一开始每一门考试都是$0$分，那么与另一个人的差距就是$tot=\sum_{i=1}^{n} b_il_i$，如果把第i门课学到满分可以缩小的差距就是$v_i=(x-b_i)u_i+b_il_i$，那么以$v$从大到小排序，贪心的选择能缩小差距最大的那门考试，将初始的差距减去$v_i$

那么就是以$v$从大到小排序之后，找到最大的$k$，使得$\sum_{i=1}^{n}v_i\leqslant tot$

但是因为不一定能把初始差距减到$0$，那么就需要有一门考试来处理这个边角料，考虑枚举这个考试，分两种情况讨论

如果当前枚举的考试在前$k$个中，那么将当前枚举到考试替换成第$k+1$个考试满分，然后判断计算当前这个考试需要的时间

如果不在，直接计算即可

考虑如何计算一个考试需要花费的时间，如果边角料少于$b_il_i$那么重要度设为$l_i$，否则就设为$u_i$

我看到网上和官方题解都是带了一个二分时间，其实并不需要

还有我一开始写的程序被corner case卡WA了，需要特判所有$b_i$都是$x$的情况

[代码](https://atcoder.jp/contests/agc034/submissions/15716166)

# AGC47C Product Modulo

这种题AT很少见啊

看到这个式子会发现一般的处理方法很难进行处理，我比赛的时候打了一个整除分块，时间复杂度也假了。但可以发现这个模数给的很特殊，不是一般的$998244353$和$1e9+7$，那么从质数的性质出发

可以发现如果式子里不是乘法而是加法，那么是很好计算的，可以使用FFT或者其他的技巧计算，考虑如何把乘法变换为加法，很明显对乘数取一个$log$就可以化乘为加，那么考虑底数的选取

质数的原根$g$，保证在$1\leq i< p-1$，$g^{i}$两两不同，那么考虑用$g$作为底数，由于$p$不大，直接枚举次数，将每一个数取对数，然后用FFT加速加法运算（在每个数对应次数的位置$+1$，将这个多项式自乘$1$次）

[代码](https://atcoder.jp/contests/agc047/submissions/15796955)

# AGC47D Twin Binary Trees

一个由i,j两个叶子节点组成的简单环是i到j在树上的路径和p[i]到p[j]树上的路径组合起来的，如果记录一下每一个叶子节点到根路径上节点标号的乘积$fac$，那么i到j的路径上的节点标号乘积就是$\frac{fac_ifac_j}{LCAfac[father_{LCA}]}$

考虑枚举每一个LCA，分别来自每一个节点左右子树的叶子节点组成的路径的LCA是当前节点，那么在第一棵树上的乘积很好计算，直接枚举左右子树的叶子节点。对于第二棵树，考虑先遍历左子树，对于叶子节点$i$，在$p[i]$到根节点的路径上每一个节点都打上标记，标记上记录通过这个点的所有路径的乘积之和，这里的乘积要乘上第一棵树的贡献，枚举右子树的所有叶子节点，也在第二棵树的上遍历，遍历到的点记录的权值减掉上一次遍历到的点乘上当前标号，就是左子树的叶子节点和这个节点LCA的乘积之和，累计答案即可

[代码](https://atcoder.jp/contests/agc047/submissions/15805348)

# AGC24D Isomorphism Freak

首先需要发现如果要将两棵子树变成同构的话，所需要的最小的颜色数是两个子树深度的较大值，并且最小的叶子节点数就是每一层节点儿子数的最大值的乘积（因为要保证每一层都相同，而又不能删去节点，那么只能把这一层所有的节点的儿子都加到最大值，最后每一层乘起来就是叶子节点数）

那么考虑树的直径，当根定在树直径的中点时，所有子树深度的最大值不超过直径的一半$+1$

那么分类讨论

如果树的直径是奇数，那么直径的中点是一条边，那么以这条边左右两个端点分别作为两棵子树的根，统计每一层儿子数的最大值，在最后合并一下即可

如果树直径是偶数，那么直径的中点是一个节点，那么直接定这个中点为根，利用之前的做法统计出叶子节点个数，但是样例告诉我们还有另一种情况存在，由于此时直径为偶数，使直径$+1$并不会影响最小染色数，所以可以把直径$+1$，然后以边作为根，套用奇数的做法即可，最后取最小值即可

[代码](https://atcoder.jp/contests/agc024/submissions/17269547)

# ARC105C Camels and Bridge

刚D去了比赛时候没想出来

由于$n$给得非常小，所以考虑枚举每一种骆驼的排列方式，那么对于一种固定的骆驼排列，可以发现由于每一只骆驼都要走过桥，在桥上的一定是骆驼排列中的一段区间，对于每一个区间两个端点的骆驼之间的距离是有限制的。具体地，对于区间$[L,R]$，$x_R-x_L\geq max(l_k)(v_k<\sum_{i=L}^{R} w_i)$，这个式子可以在m中二分找出答案，考虑如果满足这所有的条件，那么这种排列骆驼距离的方式一定合法。其实这个只要$L$向$R$连一条权值为$max(l_k)$的有向边，那么原问题就是找DAG上最长路（因为最长路保证了$u->v$所有其他的限制都被满足了），$dp$即可

复杂度$O(n!n^2logm)$

[代码](https://atcoder.jp/contests/arc105/submissions/17369337)

# AGC46D Secret Passage

首先发现这个操作可以产生一些自由的$0$和$1$，这些可以任意的插入后面没有被操作影响到的串中，并且后面没有被操作影响到的串一定是原来串的后缀，那么记$ok[i][j][k]$为前$i$位被操作所影响到产生了$j$个自由$0$，$k$个自由$1$的状态是否合法，转移的时候枚举当前选出来的两个数，含多少个自由的$0$和多少个自由的$1$（0/1/2个），那么剩下的就是原来字符串里的$0,1$

然后再考虑将这些产生自由的$0,1$放到后缀里的方案数，记$dp[i][j][k]$表示$[i,n]$这个后缀未被操作所影响到，有$j$个自由$0$，有$k$个自由$1$插入其中的方案数，由于这个后缀一定是将$0,1$插入后得到串的子序列，那么考虑用子序列匹配的方式进行转移，就是$dp[i][j][k]$可以有$dp[i+1][j][k]$转移过来，然后如果当前这一位为$0$，可以向里面插$1$，可以有$dp[i][j][k-1]$转移过来，反之亦然

那么很自然的想，只要这个状态合法，就直接累计到答案中去，但这样会算重复，由于当后缀不断变成时，更长的后缀的方案一定被包含在短后缀的方案中（因为只要按顺序填$0,1$），那么从后往前扫描，每次有一个状态合法，将这个状态累计到答案中，之后把在这个基础上向前延伸的后缀的状态设为非法即可

[代码](https://atcoder.jp/contests/agc046/submissions/17383831)

# AGC45C Range Set

好题，想了好久才想明白

首先0,1是对称的，可以把初始串变为全1，然后交换$a,b$，这个问题和原来的问题等价，那么可以通过这个性质保证$a \leq b $

然后考虑什么样子的串是合法的，首先发现如果有一段连续的1的个数$\geq b$那么通过这一段1，可以向左右两边扩展，考虑在扩展的过程中要添加一段连续的字符，如果这个长度大于对应的限制，那么直接赋就行了；如果小于，先将这一段赋上，然后会有一段覆盖到之前的已经固定的字符，由于之前第一次扩展保证合法，并且赋0操作不会把这个初始的$\geq b$的区间全部覆盖，不断用之前的步骤调整回来就可以。那么通过这种操作一定可以把除了这个初始1串的其余所有位置所有可能性统计到，再考虑这个$\geq b$的1串（当然最终可以得到的串可能不止一个$\geq b$的1串，此处只以一个举例），由于$a \leq b$，那么可以在结束扩展之后在这个1串里面操作出某几段$\geq a$的0串，这样的得到的串一定也是合法的

那么我们先假设把所有$\geq a$的0串都赋成1，最后把原来的方案数乘上去即可。考虑DP，由于直接DP不方便，那么考虑反面

首先可以通过一个DP求出所有长度为n的1串可以被$\geq a$的0串分割成多少种不同的串（注意这里要保证开头和结尾一定为1，否则会和左右两端其他的0合成更长的，这样会重复统计，但对于前缀后缀的1串，只要保证一段为1就可以了，另一端是0或1都可以），设$f[i][0/1]$表示长度为$i$的1串被分割后最后一位为$0/1$的方案数，初始条件$f[0][0]=1,f[0][1]=0$，保证第一段不为0

然后用$dp[i][0/1]$表示第$i$位为$0/1$方案数，转移的时候$0/1$最长长度只能为$a-1,b-1$，然后判断一下1串是否为前缀后缀，如果是那么系数为$f[len][0]+f[len][1]$，否则为$f[len][1]$

[代码](https://atcoder.jp/contests/agc045/submissions/17407911)

# AGC44C Strange Dance

原来AGC也有数据结构题

考虑维护一个三进制Trie，对于S只要打标记交换1,2的位置，对于R，更新时先把当前节点的0,1,2换成2,0,1的顺序，然后考虑进位，递归更新2即可

[代码](https://atcoder.jp/contests/agc044/submissions/17428631)

# AGC40C Neither AB nor BA

发现如果直接按题意去分析不能删AB或BA的性质，是很难的。由于不能删的是相邻的两个字母，那么考虑将偶数位的A改为B，B改为A，然后会发现题目变成了不能删AA或BB，那么只要A或B在序列中出现大于$\frac{n}{2}$次就一定不行，那么最终答案为$3^n-2\sum C_{n}^{i}2^{n-i} $

[代码](https://atcoder.jp/contests/agc040/submissions/17502404)

# AGC16F Games on DAG

首先考虑每一个点求出其$sg$函数，只要$sg(1)=sg(2)$那么Alice必败，否则Alice必胜，那么先统计$sg(1)=sg(2)$的数量（因为数量少）可以发现，如果两个点的$sg$函数相同那么这些点之间一定没有边相连，那么就可以把点根据其$sg$函数值进行分层，并且要保证$1$号点和$2$号点要在同一层内，

由于n非常小，那么用状压来记录状态和枚举转移，记$dp[mask]$为当前$mask$点集已经在$sg$分层中的方案数，然后更新时枚举其子集作为最后一层$sg$的点，考虑具体如何转移，由于$sg$值较大的点一定要连向比其小$sg$层中至少一个点，那么枚举的当前子集，一定有其他已经在集合中的点连向它，那么计算出原来已经在集合中的点连向这些枚举的点的边的数量$w$，那么方案数就是$2^w-1$，然后当前枚举$sg$最小的这些点之间是不能连边的，但是可以连向$sg$函数值比它大的点，同样统计出边的数量，方案数为$2^w$

最终答案就是$dp[2^n-1]$

[代码](https://atcoder.jp/contests/agc016/submissions/17677131)

# AGC2F Leftmost Ball

一开始搞出了$n^3$的dp，但并没有什么用

这种放球不同的序列并且放球之间有限制，那么可以看作有向图的拓扑序，用连边来表示限制

考虑单独领出每种颜色的第一个球，在放完这个第一个球之后才能放这个颜色其他的球，然后再限制要按顺序放不同颜色的第一个球，然后最终算出答案后乘上n!即可

那么可以构建出这样一个图，用dp求解其拓扑序数，当解锁新的一列时，将这一列中的球穿插到还没有解锁的球中，组合数计算可以得到

<img src="D:\Blog\image\1614195-20201027211936775-1132558124-1614524228355.jpg" style="zoom: 50%;" />

<img src="D:\Blog\image\1614195-20201027212123256-523697460.jpg" style="zoom:50%;" /> 

[代码](https://atcoder.jp/contests/agc002/submissions/17681492)

# AGC48D Pocky Game

首先分析可以发现，如果某一个人当前的石子数大于剩余的石子数，那么这个人一定必胜

那么可以得到两个人的策略，要么直接放弃当前这一堆石子，去抢后面更大的石子，要么一个一个拿走当前堆中的石子，以拖延对手的时间

在这种策略下取不会出现取多个石子，却不取完当前石子的操作，因为这样会缩短拖延的时间

可以设$f[i][j][k]$表示区间$[i,j]$的石子，最左端的石子个数为$k$是否可以胜利

由于对于$k_1>k_2$，如果$k_2$可以获胜，那么$k_1$一定也可以，因为可以完全复制于$k_2$一样的操作

那么可以压缩状态，设$f[i][j]$表示$[i,j]$区间内，获胜所需最小左端石子数

同理$g[i][j]$表示需要获胜所需右端的最小石子数

考虑转移，以左端为例

如果$a[j]<g[i+1][j]$，那么左端的人直接放弃$a[i]$就可以获胜，那么$f[i][j]=1$

如果$a[j] \geq g[i+1][j]$，那么两个人开始拖延时间，各自都取一个石子，直到$a[j]=g[i+1][j]$时，右端的人此时必须放弃$a[j]$，否则会导致上面那种情况的产生，那么现在区间变成了$[i,j-1]$，那么先手获胜最小石子数$f[i][j-1]$，由于之前拿走了$a[j]-g[i+1][j]+1$个石子，那么$f[i][j]=f[i][j-1]+a[j]-g[i+1][j]+1$

$g$的转移同理

[代码](https://atcoder.jp/contests/agc048/submissions/18016582)

# AGC30D Inversion Sum

这种套路还是没有完全理解啊

考虑设$dp[i][j]$表示$a_i>a_j$的方案数，然后按输入的操作进行转移，但是直接转移，不仅需要考虑$i,j=x[i],y[i]$的情况，还要把所有其他$i,j$方案数乘$2$，这样每一次是$O(n^2)$转移

注意到$i,j$不等于$x,y$的情况，每一次操作转移的瓶颈在这里，但是每一次操作都很简单只是对方案数$*2$，那么考虑将所有的$2$提取出来，最后算答案的时候再乘上去$2^q$

那么这样$dp$的意义就变为了概率

如果操作没有影响到$dp[i][j]$那么，$dp[i][j]$的概率就不变

那么只要考虑$i,j$等于$x$或$y$的情况，每一次转移$O(n)$

[代码](https://atcoder.jp/contests/agc030/submissions/18111981)

# [AGC16A,B,C,D,E,F](https://www.cnblogs.com/huangchenyan/p/13966282.html)

第一次半个下午半个晚上靠自己把大半场做完了（虽然F之前做过看了题解，D讨论了一下）

##  A.Shrinking

签到题，首先暴力枚举剩下字符串的字母是哪一个，然后可以将每一次从$n$到$n-1$的操作转化为$n+1$的位置上有一个当前枚举的字符，然后操作数就是由单个字母隔离出来的连续段长度的最大值，总体记录最小值即可

[代码](https://atcoder.jp/contests/agc016/submissions/18063369)

## B.Colorful Hats

大力分类讨论，首先一种特殊情况需要特判掉，就是所有数都是$n-1$，这样一定合法。设帽子的种类有$m$种，然后考虑由于每一种帽子的数量要么$>1$要么$=1$，如果当前说话的猫戴的帽子种类数量是$>1$的，那么$a_i=m$，如果$=1$，那么$a_i=m-1$，可以发现，$a$最大最小值不能超过$1$

如果$MAX=MIN$，那么所有$m$种帽子的数量$>1$，那么$Yes$的条件为$2m \leq n$

如果$MAX=MIN+1$，统计一下$MIN$的数量$cnt$，如果$>MAX=m$，那么不合法，否则合法条件为$cnt+2(m-cnt) \leq n$

[代码](https://atcoder.jp/contests/agc016/submissions/18063243)

## C.+/- Rectangle

构造题，受到样例的启发，我们可以这样构造

对于$(x,y)$，若$x\%h=0$且$y\%w=0$那么$a_{x,y}=-(h*w)$，否则$a_{x,y}=1$

这样可以保证所有$h*w$的子矩阵一定为负，如果总和为正那么可行

但这样会错几个点

考虑如果$h \nmid n$或$w \nmid m$，那么在最后几列最后几行，会有一些全由正数构成的行列，那么由于需要保证总和$>0$，那么要让正数尽量大，设$lim$为可以填最大数,$lim=\frac{10^9}{hw}$

那么，对于$(x,y)$，若$x\%h=0$且$y\%w=0$那么$a_{x,y}=-((h*w-1)*lim+1)$，否则$a_{x,y}=lim$

[代码](https://atcoder.jp/contests/agc016/submissions/18063088)

## D.XOR Replace

首先可以发现，对于题中的操作，设$s=Xor_{i=1}^n a_i$，如果第一次选出来的是$a_i$，那么操作完之后，下一次操作的$s=a_i$，那么就可以转化题意为现在有$n+1$一个数，第$n+1$个称作为游离在外的数，那么每一次操作等价于在$n$个数中选出一个数，并将这个数和游离在外的数交换，问是否可以得到b数组

首先可以简单的判断掉无解的情况，然后考虑如何用最小操作次数来完成这一目标

考虑对于每一个$a_i\neq b_i$，连一条$b_i\rightarrow a_i$的边，那么现在问题转化为一开始从$s$出发，至少需要添加多少条边使得图中存在一条欧拉路径，并且这条欧拉路径从s出发

由于图不一定联通，如果联通块中都存在一个欧拉路径，那么我们就可以将这些联通块串成一个链，走出欧拉路径，这一部分的代价是联通块数量$-1$（注意这里需要剔除单个点的情况，但对于出发点单点的情况，需要将出发点单独视为一个连通块）

那么现在考虑对于一个联通的图如何计算。对于一个欧拉路径，如果从终点向起点连一条边，那么图中就存在欧拉回路，而计算形成欧拉回路至少需要多少条边很好计算，即$\frac{\sum abs(out_i-in_i)}{2}$，最终$-1$即可

[代码](https://atcoder.jp/contests/agc016/submissions/18062586)

## E.Poor Turkeys

题中的概率都是吓唬人的。。。考虑转化题意，对于一对火鸡$i,j$其最终存活的概率$>0$的充要条件是存在一种吃火鸡的方式，使得最终$i,j$都存活下来

先对于题目中每一个人的选择建一条$x\rightarrow y$的边

考虑枚举$i,j,$对于$i,j$直接相连点，这些边在选择的时候必须选$i,j$相邻的那些，再考虑下去如果要保证在做这条边选择的时候这个点还存活需要怎么样操作，考虑每一边所代表的操作有先后顺序，那么从前往后考虑，根据刚才的解法，需要当前点需要在$x$时刻之前都不能被删，对于所有操作时间$<x$的边，都需要选择对应的那个点，来保证这个点不被删去，那么这样递归下去，直到有一个点需要的时候已经被删去了，就停止递归，判断当前火鸡对不合法，如果在递归$i,j$的过程中都满足条件，那么就是合法的

[代码](https://atcoder.jp/contests/agc016/submissions/18060924)

## F.Games on DAG

首先考虑每一个点求出其$sg$函数，只要$sg(1)=sg(2)$那么Alice必败，否则Alice必胜，那么先统计$sg(1)=sg(2)$的数量（因为数量少）可以发现，如果两个点的$sg$函数相同那么这些点之间一定没有边相连，那么就可以把点根据其$sg$函数值进行分层，并且要保证$1$号点和$2$号点要在同一层内，

由于n非常小，那么用状压来记录状态和枚举转移，记$dp[mask]$为当前$mask$点集已经在$sg$分层中的方案数，然后更新时枚举其子集作为最后一层$sg$的点，考虑具体如何转移，由于$sg$值较大的点一定要连向比其小$sg$层中至少一个点，那么枚举的当前子集，一定有其他已经在集合中的点连向它，那么计算出原来已经在集合中的点连向这些枚举的点的边的数量$w$，那么方案数就是$2^w-1$，然后当前枚举$sg$最小的这些点之间是不能连边的，但是可以连向$sg$函数值比它大的点，同样统计出边的数量，方案数为$2^w$

最终答案就是$dp[2^n-1]$

[代码](https://atcoder.jp/contests/agc016/submissions/17677131)

# [AGC11C,D,E](https://www.cnblogs.com/huangchenyan/p/13971595.html)

果然还是做不动正常难度的EF

A,B不想写

## C.Squared Graph

首先考虑怎样的两个点是联通的，对于点$(a,b)$和$(c,d)$，可以看作有两个人在点$a$和点$b$上，同时沿着原图的边走，每一次行走两个人都要走，如果可以分别走到$c,d$上，那么说明$(a,b)$和$(c,d)$联通

那么只要存在$a$到$c$路径长度等于$b$到$d$的路径长度，那么这样的两对点就是联通的，由于可以在图上随便走，那么只要考虑$a$到$b$路径是否存在奇数长度和偶数长度，$b$到$d$同理，如果有一种都存在，那么就是联通的

考虑原图上的一个连通块，如果这个图中没有奇环（可以通过走到奇环上来改变当前的奇偶性），那么不能使联通块中两点都存在奇偶长度的边（如果有奇环就是都存在），那么这个联通块就是一个二分图，同侧的点存在偶数的路径，异侧点存在奇数路径

考虑到点对的图上会产生多少个联通块，首先是两个出发点都是在一个连通块，如果二分图产生$2$个，否则产生$1$个

考虑两个出发点不在同一个连通块中，如果都是二分图产生$4$个，都不是二分图产生$1$个，否则产生$2$个

最后判断一下独立点即可

[代码](https://atcoder.jp/contests/agc011/submissions/18075009)

## D.Half Reflector

好，先来三个结论，推死了

1.对于一次放球的操作，如果第一个装置为$A$，那么只把第一个装置改为$B$；否则对于$x$个$B$，$y$个$A$的形式，即

$AAA...ABBB...B$变为$x-1$个$A$,后$y$个$B$，最后一个$A$的形式，$AA...AABBBB...BA$，并且球从右边出去

如果最后一个极长连续段为$BB...B$，那么全部变$A$即可

证明？手玩就可以了

2.对于足够多次操作之和，序列一定会由$BABABABA...$组成

因为球在通过$BA$的时候，如果$BA$处于序列的最后，那么就不会改变$BA$的状态，这样的$BA$是稳定的

当然如果$n$为奇数，最初一个装置的状态会在$A,B$中来回变化

并且在操作的过程中，稳定的$BA$一定从后往前依次出现，直到开头

3.可以发现，在操作过程中，一个$BA$形成之后，前边的序列是由原来的序列（剔除之前已经有的稳定$BA$）向左平移$2$个单位得到

证明：对于最简单的形式$x$个$B$，$y$个$A$$(x\geq 2,y\geq 2)$，记$(x)A$表示有$x$个连续的$A$

一次操作后$(x-1)A(y)B(1)A$

再一次操作后$(1)B(x-2)A(y)B(1)A$

再一次操作后$(x-2)B(y)A(1)B(1)A$

那么就是在最后产生了一个稳定的$BA$，并且向左平移一位

有了上面的三个结论，就可以做了，对于每一个形成的$BA$，可以得到需要操作几次，那么可以确定$k$落在哪一个范围内，在范围内暴力找即可

对于序列开头是$A$的，次数$+1$，并将开头改为$B$

对于开头$B$只有$1$个的情况，次数$+2$

否则次数$+3$

所以一段区间内最多$4$次操作，暴力更改即可

[代码](https://atcoder.jp/contests/agc011/submissions/18076545)

## E.Increasing Numbers

好妙啊，然而无脑贪心也是对的

一个重要的观察，对于一个Increasing Numbers，最多拆分成$9$个$111...1$相加的形式，那么如果当前这个数$n$可以拆成$k$个Increasing Numbers的话，$n$最多被拆成$9k$个$111...1$相加的形式（我是根本想不到）

那么考虑二分答案

$n=\sum_{i=1}^{9k}(10^{r_i}-1)$

$9n+9k=\sum_{i=1}^{9k}10^{r_i}$

那么只要统计$9n+9k$所有数位上数字的和$sum$，如果$sum\leq 9k$，那么合法

然而，每一次找最大的Increasing Numbers减掉也是对的，只要将每一个连续段缩成一个点，然后找到第一个下降的地方，前面所有的数抹掉，这个连续段抹掉第一个数，剩下的数$+1$继续处理，相当于每一次剪掉$xxxxx(x-1)99999...$这样的数，但细节超级多，一开始细节写挂了，以为做法假了

但是这个做法为什么是对的呢

考虑上面那个做法，如果直接利用$1111...1$去减$n$的话，就是贪心地最高位能取则就减掉最高位，但是直接做时间复杂度不对，这就是为什么上面那个做法要用二分的原因，考虑将若干的操作合并到一起，那么就得到这个贪心做法

[代码（贪心）](https://atcoder.jp/contests/agc011/submissions/18079271)

[代码（二分）](https://atcoder.jp/contests/agc011/submissions/18078530)

## F.Train Service Planning

首先假设向$0$方向的车出发时间为0时刻，然后只考虑单项路段，假设有$m$条单项路段，那么可以用$m$个区间表示这个行程

注意到由于每一次都是间隔$k$次出发，那么也就是两个方向经过第$i$个单向路段的时间区间，在模$k$意义下是不交的

考虑在站内停靠会产生什么影响，就是会将之后的后缀时间区间$+1$，由于我们是只考虑这两个方向列车代表的时间不交，而所在的值域是在模意义下的

那么我们只需要考虑这两个区间的相对位置关系即可，也就说我们可以只对其中一个方向的列车在站内停靠，而另一个方向的列车在途中不停靠

设$0$方向的列车永远从$k$的时间出发，那么其实就是从$0$出发，设$s_i$为$a$的前缀和，$p_i$为到$i$号站点，停靠时间的总和，其中$p_0$为出发时间

如果$b_i=1$，那么两辆列车在运行过程中不能相交，那么对于$0$方向的列车其运行时间为$(-s_i,-s_{i-1})$，$n$方向的列车运行时间为$(p_i+s_{i-1},p_i+s_i)$，那么只要这两个区间不交即可

那么我们可以解出满足这两个时间段不交的时候$p$的范围，并且这个范围应该是在模意义下的

那么问题就变成了，选择一个初始位置，每一次加一个数使得这个数在模$k$意义下对应的区间内，问最少需要加多少个数

对应到原问题上，每一次如果需要的在站内等候，那么一定是等到下班的列车到达时候就走掉，那么就是这个问题每一次都贪心的走到左端点

考虑$dp$，设$dp[i]$表示到$i$个限制左端点的最小代价，那么可以从后往前$DP$

$$
dp[i]=\min\limits_{l_i\notin [l_j,r_j] } dp[j]+dis(l_j,l_i)
$$


由于$dp$也是单调减的，那么这个$min$就可以去掉，变成最后一个没有覆盖到$i$的$j$

那么区间覆盖可以用线段树维护

[代码](https://atcoder.jp/contests/agc011/submissions/18225934)


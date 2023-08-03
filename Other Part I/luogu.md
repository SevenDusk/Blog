# P5763 [NOI1999] 内存分配

## 题目大意

有n个内存区域，然后有若干个分配，每一个分配的时间请求时间为T，要求$m$的连续段，运行时间为$p$

如果存在空闲位置，那么将其中地址最小的那些分配给它，否则加入队列，只要在任一时刻，存在长度为 $M$ 的空闲地址片，系统马上将该进程取出队列，并为它分配内存单元

求全部进程都运行完毕的时刻和被放入过等待队列的进程总数
$$
m\leq 10000
$$


## 算法讨论

我们使用priority_queue 来维护事件的先后顺序，存储时间、询问的编号，是结束访问还是加入访问

然后还要动态维护内存区里面空闲连续段的情况，可以使用vector存（因为n可能很大，所以存的是若干区间$[l,r]$）

当请求的时候，在vector里面枚举，找到第一个连续段长度大于要求的区间，然后将区间分裂，如果没有找到那么加入队列，计数器+1

对于删除的情况，就是向vector里面加入一段区间，由于这段区间可能跟之前之后的区间合并，那么需要特殊判断一下，更新vector内的状态

**有一点需要注意的是，由于可能有多个事件发生在同一个时间，那么需要给事件定一个顺序，那么就是先删除后插入**

还有就是队列中的事件是最优先的，需要排在**删除之后（错误点）**插入之前，如果某一个时间点没有插入操作，需要特殊处理，可以用unordered_map 判断

弱点：没有对模拟的过程拆解清楚，然后导致在顺序方面会有麻烦，同时也导致了实现速度比较慢，一开始试图用set维护这个区间，后来意识到有一种情况无法处理，只能枚举，那么set枚举还需要多$O(logn)$，才改成vector，导致花费的时间比较多，需要将每一个步骤想清楚再动手

第一次提交:Wrong answer

# P5751 [NOI1999] 01串

## 题目大意

构造一个0/1串，使得任意长度为$L_0$​​的子串，$0$​​ 的个数大于等于 $A_0$​​ 且小于等于 $B_0$​​，任意长度为$L_1$​​的子串，$1$​​ 的个数大于等于 $A_1$​​ 且小于等于 $B_1$​​

输出满足所有条件的 01 串中 1 的个数的最大值
$$
n\leq 1000
$$

## 算法讨论

首先考虑将约束条件写出来，设$s_i$为1的前缀和
$$
a_1\leq s_i-s_{i-l_1}\leq b_1\\
l_0-b_0\leq s_i-s_{i-l_0}\leq l_0-a_0\\
0\leq s_i-s_{i-1}\leq 1
$$
然后用这些不等式建差分约束系统

**需要注意的是，需要求的是最大值，那么对每一个位置的约束都要达到上界（错误点），就要第一个位置达到上界，然后后面的就可以跟着达到上界（虽然是最短路），在本题中并不需要特殊判断，只要是1的前缀和即可（而不是0的前缀）**

弱点：对于差分约束系统的特殊解和一般解的关系不明确

第一次提交:Wrong answer

# P5757 [NOI2000] 古城之谜

## 题目大意

![img](https://cdn.luogu.com.cn/upload/image_hosting/fj2tbbqd.png)

给出以上定义和一个文章，n个单词以及词性，求在最少划分成多少个句子的时候，最少划分多少个单词
$$
n\leq 1000
$$

## 算法讨论

可以考虑对所有单词建一个字典树，然后最简单的想法就是对于每一个位置都找出以这个位置开头可以形成哪些单词，然后利用这些位置进行DP

由于每一个单词长度$\leq 20$，直接暴力枚举复杂度就是对的

还需要注意到句子的数量等价于两个相邻名词短语的数量，那么只需要记录之后一个短语是名词还是动词就可以算出句子的数量

设$dp[i][0/1]$​表示i开头是名词/动词短语的答案（一个二元组），然后从后往前转移，如果当前是名词，上一个位置也是名字，句子数量+1，如果是动词上一个位置就不能是动词，如果是辅词，那么继承上一个位置的信息

复杂度$O(20len(s)+n)$​

弱点：需要快速注意一些小的数据范围

第一次提交:Accepted

# P5758 [NOI2000] 算符破译

## 题目大意

现在一共有13种符号$0,1,2,3,4,5,6,7,8,9,+,*,=$，这些符号被替代成$a-m$这个13个小写字母，给出n个表达式（其中一定包含一个等号，并且等式成立），求出一定可以确定哪些对应关系
$$
n\leq 1000\\
5\leq len\leq 11
$$

## 算法讨论

显然是搜索还有模拟，主要是如何进行剪枝

搜索一个重要的剪枝就是把可能状态数小的放在最前面搜索，那么可以先枚举$=,+*$这个三种特殊符号的对应关系，那么也就确定了哪些是数字的段数

接下来一个一个串的进行枚举，并且只枚举在这个串中出现过的并且之前没有出现过的字母对应哪些数字，**需要注意的是如果一段数字最前面是$0$​​​，那么是不合法的，同时如果只有一个数字那么又是合法的**（错误点）

在更新答案的时候可以将对应关系存到vector里面，然后输出答案的时候，枚举左右两边的位置，如果在vector每一组中都出现了，那么就合法输出（错误点）

至于表达式计算，那么就可以先提取出+号，然后再在分成的每一段中提取出*，依次进行计算即可

还有剪枝就是，需要求出等式左右两边最大可能的位数和最小可能的位数

还有就是如果无法再进行更新，那么就退出（错误点）

弱点：对于搜索过程的剪枝和计算的特殊地方没有很快的处理好（也是一些细节没有考虑到就开始写，写完之后检查也没有查出来），同时对于较长代码（200+）的整体把握能力不够

第一次提交：TLE

# P5747 [NOI2004] 曼哈顿

## 题目大意

P 城有 $m$ 条横向街道和 $n$ 条纵向街道，横向街道横贯东西，纵向街道纵穿南北

现在有k条请求，从一个十字路口到另一个十字路口的最短路径的长度必须等于它们之间的曼哈顿距离，

原来每条道路原来都是有方向的，改变每条街道的行驶方向都有一定的工作量，工作量的大小因道路而异

输出是否有解，最小工作量，和构造方案
$$
m\leq 10,n\leq 100,k\leq 100
$$

## 算法讨论

首先$m\leq 10$，那么考虑枚举每一个横向的走向，对于每一组询问如果在横向之间每一条是跟要求方向同向的，那么这组就是不合法

接下来考虑如何处理纵向的道路

设$(a,b)$为出发点，$(x,y)$为目标点

- 如果$a,x$两条道路与目标方向同向，那么在$b,y$​至少存在一条纵向道路，跟目标方向同向
- 否则如果$a$​道路不同向，那么$b$必须是同向道路，如果$x$道路不同向，那么$y$必须是同向道路

那么接下来就可以转化为有若干三元组$(l,r,v)$表示$[l,r]$至少有一条方向为$v$的纵向道路，那么考虑DP

设$dp(i,j,k)$​表示当前考虑到第i条纵向道路，上一个方向为$N$的纵向道路为$j$，为S的纵向道路为$k$，**事实上$j,k$中有一个会等于$i$**，那么总状态数就是$O(n^2)$的，并且第一维还可以滚动掉

转移的之前啊需要对两个方向分别先预处理出右端点在$i$​的区间的最大左端点，分别记作$p_{i,0},p_{i,1}$

转移的时候枚举$j,k$（其中之一为$i$​），如果哪一个小于对应的左端点，那么把当前i改为对应的方向，如果跟与哪里的方向不同，那么就加上代价

由于还要输出方案，那么就记录一下转移过来的位置，虽然滚动了第一维，但对于不同的i来说，状态都是不交的，所以并不会产生影响

时间复杂度$O(2^m(n^2+k))$​

弱点：对于常数分析的敏感度，还有特殊性质的敏感度，一开始想写$O(2^mn^3)$，觉得表面上过不了，但是常数非常小，可以在这个做法差不多的时间内过去，本身这个做法也是在实现$O(2^m n^3)$​做法的时候发现的

第一次提交:Wrong Answer

# P5759 [NOI1997] 竞赛排名

## 题目大意

每个选手要参加数学、物理、化学、天文、地理、生物、计算机和英语共八项竞赛,最后综合八项竞赛的成绩排出总名次。 设 $x_{i,j}$分别表示编号为 $i$ 的选手第 $j$项竞赛的成绩
$$
avg_j=\frac{1}{N}\sum\limits_{i=1}^nx_{i,j} (1\leq j\leq 8)\\
sum_i=\sum\limits_{j=1}^8 x_{i,j}\\
if \sum\limits_{i=1}^n |x_{ij}-avg_j|=0,y_{i,j}=0
\\else\ y_{i,j}=\frac{x_{i,j-avg_j}}{\frac{1}{n}\sum\limits_{i=1}^n |x_{ij}-avg_j|}
$$

1. 总位置分高的选手名次在前；

2. 若两个或两个以上的选手总位置分相同，则总分高的选手名次在前；

3. 若两个或两个以上的选手总位置分和总分均相同，则编号在前的选手名次在前。

   求出最后排名

   

## 算法讨论

按照上面的式子求出对应的变量位置分，总分，编号，然后将放到一个结构体里面，自定义cmp函数，需要利用浮点数比较，设eps用sgn函数比较正负，然后输出排名

第一次提交:AC

# P5761 [NOI1997] 最佳游览

## 题目大意

给出一个 $m\times (n-1)$ 的矩阵，连续的 x 列中每列选取一个数，要求所有选取的数的和 **最大**，求出这个最大和。

## 算法讨论

将一列中最大值求出，然后求出这最大值序列的最大子序列的值

就是利用dp就可以求出，设$f(i)$到$i$的位置的答案
$$
f(i)=\max(f(i-1)+a_i,0)
$$

# P5753 [NOI2000] 瓷片项链

## 题目大意

给定了泥土的总体积和烧制单个瓷片的损耗，烧制的瓷片数不同，能够得到的项链总长度也不相同，请计算烧制多少个瓷片能使所得到的项链最长。

## 算法讨论

枚举每一个瓷片所用的体积，求出最大值，查看是否出现过即可

需要避免浮点计算，利用$i(v-v_0i)$比较即可

# P5755 [NOI2000] 单词查找树

## 题目大意

给出一些字符串，然后问建出的字典树有多少个节点

## 算法讨论

将字符串插入字典树即可

# P5760 [NOI1997] 积木游戏

## 题目大意

要从n块积木中选出若干个积木，然后分成编号连续的$m$组，每一组需要按照编号顺序叠成一堆，每一个下面的块需要将上面块的上表面的完全覆盖
$$
n,m\leq 100
$$


## 算法讨论

设$dp(i,j,k)$表示当前最后一块积木是第$i$号，然后已经分成了$j$组，$i$积木的状态是$k\in \{0,1,2\}$，

转移的时候需要枚举上一块积木$q$在哪里，和上一块积木的状态$p$，如果上一块积木的状态可以放在这块积木的下面，那么就有
$$
dp(i,j,k)=dp(q,j,p)+h_{ik}\\
dp(i,j,k)=dp(q,j-1,p)+h_{ik}
$$
复杂度$O(n^3)$
#  Petrozavodsk Winter 2020. Lviv NU Contest. D [Split in Sets](https://www.acmicpc.net/problem/18733)

## 题目大意

现在有$n$个不同的球，每一个球上面有一个数$a_i$，现在有$k$个不同的盒子，要将这$n$个球放到这$k$个盒子中，盒子不能空，假设一个盒子的价值为盒子里面球$a_i$按位与的结果，要求最大化所有盒子价值之和，并且求出方案数
$$
k\leq n\leq 10^5
$$

## 算法讨论

我一开始一直在想wqs二分，这个只能解决第一问没法解决后面那一问（因为如果出现了三点共线的情况，计算的方案数是线段端点处的方案数而不是所求k的方案数）

一种简单的想法是从高位向低位进行考虑，假设当前这一位为$r$

由于我们要最大化盒子的价值和，直接的想法就是最大化这一位上为1的盒子个数，证明考虑当前有最多可以有$p$个盒子当前这一位为1，盒子价值分别为$b_1,...,b_k$，现在考虑只有p-1个盒子当前这一维为1，调换的盒子为$i,j$（以两个盒子为例，其他情况都是可以归到2个盒子上的），原来$b_i$第r位为1，$b_j$第$r$位为0，现在使得两个盒子第r位都为0，那么最坏情况就是原来$b_i,b_j$第$r-1$位都为0，调整之后$b_i',b_j'$第$r-1$位都为$1$，显然$b_i+b_j\geq b_i'+b_j'$

然后需要考虑一个递归算法，设$f(a_1,a_2,...,a_n,k)$表示这个问题最大价值和，当前考虑第$r$位

- 如果$a_i=0,\forall i\in[1,n]$，答案返回0，方案数为$\left\{\begin{matrix}n\\k\end{matrix}\right\}k!$
- 如果所有数第r位都为1，那么返回$k2^r+f(a_1-2^r,...,a_n-2^r,k)$，方案数为递归考虑下去的方案数
- 假设$n$个数中第$r$位为1的个数有$p$个，如果$p<k$，那么把这$p$个数一个数一个盒子放，然后将剩下的数递归考虑，递归考虑下去的方案数$\times A_p^k$
- 如果$p\geq k$，那就需要将所有这一维为0的数放到同一个盒子里面，相当于是将这些数与起来得到一个新的数，这一维为1的数不变，k不变递归考虑，贡献为$(k-1)2^r$，方案数为递归下去的方案数



**这道首先需要推出那个当前位让1尽可能多的结论，然后再需要考虑一个递归算法，平时接触的比较少，需要着重强调一下（事实上还是DP的思想，但不过递归算法还可以解决构造等非最优化/计数问题）**

# Belarusian SU Contest D [Data Structure Problem](https://www.acmicpc.net/problem/18572)

## 题目大意

给定$2^p$的数组$a$，要求支持以下五种操作

单点加，区间查询

```
b = array of 2^p zeroes
for i in 0..2^p - 1:
  b[i and k] += a[i]
a = b
```

```
b = array of 2^p zeroes
for i in 0..2^p - 1:
  b[i or k] += a[i]
a = b
```

```
b = array of 2^p zeroes
for i in 0..2^p - 1:
  b[i xor k] += a[i]
a = b
```

$$
p\leq 19,q\leq 5*10^5
$$

## 算法讨论

### Solution 1

首先用线段树维护这个数组，然后考虑一位一位拆开后三种操作

其中And 1,Or 0是可以忽略的，值得注意的是，如果出现了And 0，Or 1的操作，会发现所有数的位置在这一维上都会变成相同的，那么也就是说这一维是确定的，并且只会被确定1次，那么对于这样的修改可以暴力修改数的位置

我们可以维护一个$big_i$表示经过And 0 Or 1操作后，这一维大部分数为0还是1，在加入一个新的数时，如果加入位置这一位不等于$big_i$，那么就需要在这一位上记录加入的位置，当下一次And 0 Or 1操作的时候，将这一位加入到$big_i$中，可以发现最多只会被修改$p2^p+qp$次，由于是用线段树维护的，那么还需要乘上p，时间复杂度就是$O(p^2 2^p+qp^2)$但是跑不满

还有就是Xor操作，这个只需要打标记，如果为1，那么就需要交换左右儿子，还需要注意一下修改记录的位置

### Solution 2

大体内容跟1类似，但是是将左右儿子进行合并，这样的操作就是$O(p2^p+qp)$，具体实现的细节需要避免空子树的合并，这就需要用一个队列维护当前需要的合并的节点



**当对一棵0/1字典树进行and,or操作的时候，可以考虑将左右儿子进行合并，这在最大化AND，Or中也是可以应用的**

#### 最大化AND,OR

当操作是XOR时可以简单地利用Trie来进行维护，但当是AND的时候，由于当去匹配的数当前这一位是0时，不知道应该是走0还是1，如果两个子树都走下去的话，复杂度不对，那么考虑将1子树合并到0子树上，然后查询的时候如果当前这一位是1那么走1子树，如果是0就走向0/1子树。这样单次询问就是$O(m)$，预处理为$O(m2^m)$，$m$为位数，适用于$m$比较小的情况

# Petrozavodsk Winter 2017. Tsinghua U Deep Dark Fantasy Contest. C [Eulerian Orientation](https://www.acmicpc.net/problem/19394)

## 题目大意

给定$n$个点$m$条边的无向图，要求选出若干边，使得子图为欧拉图（存在欧拉回路，不要求联通），如果选出了x条边，其贡献为$x^2$，求贡献之和
$$
n,m\leq 2*10^5
$$

## 算法讨论

**考虑一条欧拉回路是可以拆成若干环，使得两个环的边之间没有相交，考虑dfs树上返祖边形成的环是无向图中所有环的基（这里的运算是异或），那么选择选择返祖边的方案恰好与选出边组成欧拉回路的方案一一对应**

那么问题就转化为了，选择若干返祖边，然后将对应环上的树边异或，最终的方案就是选择的返祖边和被异或奇数次的树边组成，求所有方案边数平方的和

考虑$x^2$的贡献相当于在方案中选出两条边的方案，答案等价于求出包含选定两条边的方案数之和

首先如果一条边会出现的在某一个方案中，至少需要有一个返祖边覆盖了这个树边，假设有$k$条，返祖边有$A$条

- 如果选出的两条边都是树边

  - 如果选出的两条边是同一条边，方案数为$2^{A-1}$
  - 如果是不同的边，经过讨论会发现，如果覆盖这条树边的返祖边集合相同，那么方案数为$2^{A-1}$，否则为$2^{A-2}$，对于如何求出返祖边集合，那么需要用集合hash，对每一条返祖边随机一个64位权值，然后树边的权值就是覆盖这个边返祖边的权值异或和（树上差分实现），假设划分出来的等价类大小为$|s_1|,|s_2|,...$

  总的方案数就是$k^22^{A-2}+\sum |s_i|^2(2^{A-1}-2^{A-2})$

- 如果选出来的两条边都是返祖边，那么方案数$A(A-1)2^{A-2}+A2^{A-1}$
- 如果选出来的两条边一条边是返祖边一条树边，假设第i条返祖边的覆盖了$len_i$条树边，并且这些树边中只被第i条返祖边覆盖的有$cnt_i$条
  - 如果选择的树边没有被第i条返祖边覆盖，方案数为$(k-len_i)2^{A-2}$
  - 如果选择的树边只被第i条返祖边覆盖，方案数为$cnt_i2^{A-1}$
  - 如果选择的树边不止被第i条返祖边覆盖，方案数为$(len_i-cnt_i)2^{A-2}$



**欧拉子图的个数等于$2^{n-m+c}$，c为图的联通块个数，n-m+c相当于就是返祖边的个数，像这种欧拉回路的问题可以考虑转化为dfs树，变成树上问题**

# XVIII Open Cup. Grand Prix of Ukraine. E [Message](https://official.contest.yandex.ru/opencupXVIII/contest/5917/problems/E/)

## 题目大意

给定两个字符串$s,t$，现在可以从s中删除某一个字符的最开头位置或者最后一个位置，每一个位置都有一个权值，删去一个字符代价就是这个位置上的权值

现在要从s变成t，问最小需要多少代价
$$
|s|,|t|\leq 2*10^5
$$

## 算法讨论

首先转化为保留s中的权值和尽可能大，考虑以下一个暴力DP，设$dp(i,j)$表示$s$中匹配到第$i$个字符，$t$中匹配到第$j$个字符的最大权值和，有如下两种转移，假设字符$v$在t中第一次出现的位置在$first(v)$，最后一个位置出现在$last(v)$
$$
dp(i,j)=dp(i-1,j-1)+c_i,s_i=t_j\\
dp(i,j)=dp(i-1,j),j<first(s_i)\ or\ j\geq last(s_i)
$$
如果我们按照$first,last$将$t$划分成若干段的话，每一段中每一种字符是走哪种转移都是确定的，也就是说在这一段中对应的$s$中必须要保留的字符也是确定的

由于这样的段数最多只有52段那么就考虑在$O(|s|)$的时间内完成这一段的转移，只保留s中字符满足在当前段在$first(v),last(v)$之间的字符，那么相当于是要选出一个子串，使得这个子串与t中当前段相同，并且要求权值和最大

这个可以通过KMP来实现，假设当前保留的字符第i个位置对应原串中第$id(i)$个位置，当前匹配的位置为i，权值和为sum，那么有$dp'(id(i))=\min\limits_{j=id(i-m')}^{id(i-m'+1)-1}dp(j)+sum$

需要注意的是，这里还是可以转移到$[id(i),id(i+1)-1]$的位置上的，相当于打一个标记，然后去更新

然后对于$first,last$的位置，就直接用上面的式子进行转移即可

时间复杂度$O(|S|+|T|)$



**一开始想了一个假做法，应该最初先推n^2的DP，然后在这上面进行优化，这种整段转移的优化，在对于转移不多的情况下（并且条件不复杂）的情况，可以很好的优化DP**

# Petrozavodsk Winter 2017. Tsinghua U Deep Dark Fantasy Contest. A [Random Numbers](https://www.acmicpc.net/problem/19392)

## 题目大意

Chihiro 独立且等概率生成了 $n$ 个在 $[1, 10^{18} ]$ 中的随机整数$a_1 , a_2 , · · · , a_n$
Chihiro 选择了一个整数 $m$，接着生成了一个在 $[0, m)$ 中的随机整数 $k$，最后令 $b_i = (a_i + k)\mod m$，并随机打乱得序列 $\{b_i\}$，给定序列 $\{a_i \}, \{b_i \}$，请求出一组合法的 $m, k$
$$
10^5 \leq n\leq 2*10^5,b_i<10^{10},k<m\leq 10^{10}
$$

## 算法讨论

这道题用到了很多随机的性质，很好的一道题

**首先由于$a_i$是随机生成的，那么说明$a_i\bmod  m$分布是均匀的，那么$(a_i+k)\bmod m$也是均匀分布的**，注意到$m\geq \max b_i$，那么说明$(\max b_i,m)$这个区间内是不存在$(a_i+k)\bmod m$分布的，假设$r=\max b_i,d=m-r$，概率就为
$$
p=(\frac{r}{r+(m-r)})^n=(\frac{r}{r+d})^n=(\frac{1}{1+\frac{d}{r}})^n
$$
**这个概率随着$r$增加而增加，那么最坏情况就是$r=10^{10}$，可以发现当$d=O(n)$时候，$p=0.1$到$0.01$之间了，那么可以说明可能的$d$，最多只有$O(n)$个**

那么枚举$m$，剩下的就是如何check一个$m$是否合法

假设$b_i$对应着$a_{p_i}$，那么如果对于$\forall i,j$使得$b_i-a_{p_i}\equiv b_j-a_{p_j}(\bmod m)\Leftrightarrow b_i-b_j\equiv a_{p_i}-a_{p_j}(\bmod m)$，可以发现如果满足$b_i-b_1\equiv a_{p_i}-a_{p_1}(\bmod m)$的话，一定也是满足上面的条件的

考虑将所有等式加起来，那么有
$$
na_{p_1}\equiv \sum\limits_{i=1}^n a_i+b_1-b_i (\bmod m)
$$
显然右边是一个常数$c$，那么转化为解不定方程$nx+my=c$，其中$x=(a_{p_1}\bmod m)$，解出最小解$x_1$，通解为$x_1+k\frac{m}{gcd(n,m)}$，由于$x_1<m$，那么本质不同的解最多只有$gcd(n,m)$

如果知道了$(a_{p_1}\bmod m)$，那么就可以推出$k$，然后对于$(m,k)$需要check是否可以

我们可以将所有$b_i$存到一个hash表中，然后check(m,k)的时候，从1开始计算$(a_i+k)\bmod m$，如果计算结果hash表中没有就直接返回，**由于$a_i,k$是随机的，那么如果m,k不是合法的，当m比较大的时候那么计算结果是hash表中存在的数的概率是很小的，当m比较小的时候，由于$n\geq 10^5$，那么$b_i$应该覆盖整个模意义下的值域，那么检查的时候还需要记录某一个数的出现次数**

期望是$O(1)$，总的时间复杂度$O(n\log n)$

#  XX Open Cup. Grand Prix of Wroclaw. J [Planet of the Singles](https://official.contest.yandex.ru/opencupXX/contest/17756/problems/J/)

## 题目大意

给定长度为$n$的01串$s,t$，现在有以下三种操作

- 将某一位的0变成1，代价$t_0$
- 将某一位的1变成0，代价$t_1$
- 交换两个相邻位，代价$t_s$

现在要将$s$变换到$t$，求最小代价

## 算法讨论

### Solution 1

首先注意到交换两个位置$i,j$上的数（这两个位置上分别为0和1），需要交换次数就是$|i-j|$，考虑中间的0和1的连续段，往一个方向的交换，只需要经过其中一种的连续段，那么现在有两个方向，那么说明两种连续段都会要被经过，那么就是$|i-j|$

那么可以发现，如果不动已经匹配的位置一定是最优的，那么我们只需要考虑$s_i\neq t_i$的位置，将这些位置记作$a_1<a_2<...<a_k$，记$c_i=s_{a_i}$

那么问题就变成了，现在要将$c_i$变为$c_i\otimes 1$，通过0变1，1变0，交换相邻两个数的操作

#### 正确做法

考虑如果不存在前面两个操作的话，那么要求$c_1,...,c_k$中0/1的个数相同，那么就可以将$c_i=0$的数看作-1，$c_i=1$的数看作1，那么和要为0

那么如果存在前面两个操作的话，那么总体而言就不需要0,1个数相同，但是如果除去那些进行了前两种操作的位置，剩下分成的若干段一定是需要满足0,1个数相同的这个条件

那么相当于我们需要选出来若干个段，使得这些段不相交，段内进行交换，对于不属于任何一个段的位置使用前两个操作，那么就很显然是一个DP了，我们需要处理出来最短的合法段（段内不能再进行一次划分），然后将这些段进行组合

对于段间的代价可以利用前缀和进行维护，但是对于段内的操作就稍微有一点复杂

首先假设1个数-0个数的前缀和为$pre_i$，记$f_i=\sum\limits_{k=1}^i (a_{k+1}-a_k)pre_k$，那么答案就是$\sum\limits_{i=l}^{r-1} (a_{i+1}-a_i)|pre_i-pre_{l-1}|$，由于$[l,r]$是不可以再进行划分的 ，那么也就是说对于$i\in[l,r)$，不存在$pre_i=pre_{l-1}$，由于$|pre_i-pre_{i-1}|=1$，那么对于$i,j\in [i,j)$，满足$[pre_i<pre_{l-1}]=[pre_j<pre_{l-1}]$，就是段内$pre$与$pre_{l-1}$的大小关系是确定的，也就是可以将绝对值符号放到最外面

那么一段的答案就是$|f_{r-1}-f_{l-1}-(a_r-a_l)pre_{l-1}|$

就可以进行DP了，将最小段存到右端点上，设$dp(i)$表示当前考虑到$i$的最小代价，对于$r=i$的所有$l$
$$
dp(i)=\min(dp(i-1)+[c_i=0]t_0+[c_i=1]t_1,dp(l-1)+cost(l,i))
$$

#### 假做法

一开始建出了一个费用流的模型，想着模拟费用流（事实上是可行的），想只考虑0或者1，然后进行匹配，由于从小到大匹配的一定是递增的，但是可以将1变成0，0变成1，然后按照位置从小到大进行讨论

但这个不符合费用流的增广规则，费用流必须要找最短路进行增广

后面想到转化之后，想直接利用一个栈进行贪心选择，遇到与栈顶元素不同的元素，就将其匹配起来，然后对于所有的匹配对，比较直接修改和交换的代价，选择比较小的那个，对于没有进行匹配的元素直接进行修改

但是这样显然不对，当前较少的元素不一定是跟前面的元素匹配，可能是跟后面的元素匹配

比如说$a=2,3,5,6,7,c=1,1,0,0,1$，按照上面的做法就是会$5-3,6-2$匹配，事实上$3-4,6-7$匹配更优



然后进行一些改进，

# XVI Open Cup. Stage 15: Grand Prix of Moscow. K [Bipartite Graph](https://official.contest.yandex.ru/opencupXVI/contest/2366/problems/K)

## 题目大意

现在有一个左侧n个点，右侧m个点的二分图，其中存在k条边

现在不断往里面加边，第i次加边$i\bmod n\rightarrow i\bmod m$，求图联通至少需要加入多少条边
$$
n,m\leq 10^9,k\leq 10^5
$$


## 算法讨论

首先可以发现，如果一开始二分图中不存在任何边，那么最终形成的联通块是有$\gcd(n,m)$个

记$g=\gcd(n,m)$

其中所有编号$\bmod g=i$的点形成了一个联通块

证明的话，根据裴蜀定理可以简单证明

考虑存在两边点的连边$i\rightarrow j$的条件
$$
k\equiv i(\bmod n)\\
k\equiv j(\bmod m)
$$
那么$k=nx+i=my+j$
$$
nx+my=j-i
$$
那么这个二元一次方程有解的条件为$\gcd(n,m)|j-i$，那么说明$i\equiv j(\bmod \gcd(n,m))$

那么如果整张图需要连成一个联通块，那么我们就需要在输入的边中存在在$\bmod g$不同的点之间的边

显然如果$g-1>k$，那么图一定不连通

否则$O(g)=O(k)$，那么可以对于每一个联通块建一个点，用并查集判断利用$\bmod g$不同点之间的边，最终的图是否联通

如果联通块，然后再依次考虑每一个联通块之内的时间，答案就是所有联通块联通时间的最大值

将$n,m$除以$g$

首先对于$\bmod g=i$的联通块来说，加入第一条边的时间为$i+1$，之后每一条边间隔$g$时间加入

我们假设$n<m$，可以发现在前m次加入边的时候，不会产生$i\rightarrow  j,(i\bmod n)\neq (j\bmod n)$的边，那么经过m次加边之后，形成了n个联通块

假设$k=m\bmod n$

可以发现第$m+1$到$n+m-1$次加边中，其加边的顺序为
$$
k\leftrightarrow 0\\ 
k+1\leftrightarrow 1\\
...\\
n-1 \leftrightarrow n-k-1\\
0\leftrightarrow n-k\\
...\\
k-2\leftrightarrow n-2
$$
如果将左边的点数量看作k,右的点数量看作n-k，那么可以发现，这个问题变成了一个规模更小的子问题

然后处理一下之前存在边的连接点的情况

然后不断递归下去，直到$n-1\leq |E|$，在这种情况下，才有可能在前m次加边中就使得这个部分联通，那么此时的$O(n)=O(E)$，那么可以直接暴力建出一个n个点的并查集，模拟$m+1$到$n+m-1$次连边即可

如果联通块，那么在前m次加边中找最后一个需要加边的位置即可，可以从后往前枚举，枚举到第一个就停下即可

时间复杂度$O(k\log k)$
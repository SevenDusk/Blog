# XVI Open Cup. Stage 15: Grand Prix of Moscow. K [Bipartite Graph](https://official.contest.yandex.ru/opencupXVI/contest/2366/problems/K)

首先令$n<m$，那么可以发现的是在进行前$m$次操作的时候，所有$\bmod n$相同编号的右边点联通在一起，左侧的点可以只看作一个中转点

那么我们可以只保留右边的$m$个点，除了前$n$次操作将左侧点连接到右侧点之外，其他都可以看作右侧点之间进行连边，在$(n,m]$次操作中，第$i$号点连接了$i+n$号点

那么在进行$m$次操作之后所有$\bmod n$的点都可以被缩成一个点，在接下来的连边中，可以发现的是$\bmod n\equiv 0$的代表节点连向$\bmod n\equiv (m\bmod n)$的代表节点

那么可以发现是，当前我们可以将$n'=m\bmod n,m'=n$，这样就变成一个子问题，并且这个过程就是欧几里得过程，最多只会进行$O(\log n)$轮

那么接下来的问题就是考虑在初始已经加入的$k$条边，考虑当前轮的情况，操作次数从1开始编号，那么可以考虑二分第一次所有节点联通的时间

需要注意到，如果当前只考虑操作中连边形成的联通块数量的话，那么这些联通块数量-1$\leq k$，否则一定是不联通的，那么联通块数量就限制到$O(k)$级别

假设进行了$mid$次操作，那么此时联通块数量为$m-mid$，其中有$n$个联通块和编号在$[n+mid-1,m-1]$的单点

可以直接用并查集维护加入这$k$条边之后，这些点的连通性

这样的时间复杂度理论$O(n\log ^2n \alpha(n))$但常数很小

# ByteDance/Moscow Workshops Programming Camp 2020 Online Contest, G [Graph Subpaths](https://haltoj.top/d/haoba/p/155)

首先给所有边进行编号

然后对于每一个节点维护从1号节点到当前节点

# Petrozavodsk Winter 2020. Day 9: Yuhao Du Contest 7 [Fast as Ryser]([18758번: Fast as Ryser (acmicpc.net)](https://www.acmicpc.net/problem/18758))

首先可以考虑在$i,i+1$之间连一条虚边

那么由于匹配的性质，一个点最多只能匹配一个点，那么可以发现在加上虚边之后，每一个点的
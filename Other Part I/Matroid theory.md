# 1.1

一个拟阵$M$是有序对$(E,\mathcal{I})$，其中$E$是一个有限集，$\mathcal{I}$是$E$的子集的组(collection)

满足以下三个条件$I1,I2,I3$

- $\empty\in \mathcal{I}$
- 如果$I\in \mathcal{I}$，并且$I'\subseteq I$，那么一定有$I'\in \mathcal{I}$
- 如果$I_1,I_2\in \mathcal{I},|I_1|<|I_2|$，那么一定存在一个元素$e\in I_2-I_1$，使得$I_1\cup e\in \mathcal{I}$

其中第三条性质称为indep augmentation axiom

$\mathbb{I}$中的集合称为independent sets of M，E称为ground set of M



对于一个$n\times m$的矩阵，E为列编号的集合，令$\mathcal{I}$表示所有E的子集X，满足在X中编号的列向量是线性无关的，那么$(E,\mathcal{I})$就是一个拟阵

证明：显然第1，2个条件是成立的，只需要证明第三个条件成立，利用反证法

假设$I_1,I_2\in \mathcal{I},|I_1|<|I_2|$，令$W$为$I_1\cup I_2$中的向量形成的线性空间，显然W的维数至少为$|I_2|$

我们现在假设$\forall e\in I_2-I_1$使得$I_1\cup e$是线性相关的，那么说明W是被$I_1$的线性空间包含，那么说明$W$的维数$\leq |I_1|$

那么就推出矛盾

我们记$M[A]$表示由矩阵$A$推出的拟阵

对于不属于$\mathbb{I}$的E的子集称为dependent set

一个minimal dependent sets是指所有E的子集$X\notin \mathcal{I}$满足，$\forall I\subseteq X$，满足$I\in \mathcal{I}$，称为$M$的**circuit**，记作$\varrho(M)$，如果一个circuit of M包含了n个元素，那么也称之为n-circuit

显然一个拟阵可以被$\varrho(M)$唯一确定

有以下三个性质

- $\empty \notin \varrho$
- 如果$C_1,C_2\in \varrho,C_1\subseteq C_2$，那么$C_1=C_2$
- 假设$C_1,C_2\in \varrho,C_1\neq C_2$，令$e\in C_1\cap C_2$，那么一定存在一个$C_3\in \varrho$满足$C_3\subseteq (C_1\cup C_2)-e$

第三个条件称为circuit elimination axiom

## 定理 1

令$\varrho$表示集合$E$子集族，满足前三个条件，令$I$表示E的子集族表示任何一个其中的集合不包含$\varrho$中的集合

那么$(E,\mathcal{I})$是一个拟阵，并$\varrho$是其的circuit



一个命题，假设$I$是一个independent set，并且$e$是M中的一个元素，使得$I\cup e$是dependent，那么存在一个唯一的circuit被包含于$I \cup e$并这个circuit包含$e$



接下来就是关于图论的方面

假设$E$表示图G中的所有边的集合，$\varrho$表示所有环上边集合的族，那么$\varrho$是E上拟阵的circuit

证明的话对于前两个性质来说很显然，对于第三个性质，相当于是说，对于G上两个不同的环$C_1,C_2$，e是其一条公共边，那么在$(C_1\cup C_2)-e$中一定存在一个环，这是比较显然的

这样的拟阵称为cycle matroid or polygon matroid of G，记作$M(G)$

我们也可以定义两个拟阵是同构的条件，跟代数结构同构的定义类似，也是要构造一个双射$\psi:E(M_1)\rightarrow E(M_2)$，满足$\forall X\subseteq E(M_1)$，$\psi(X)$是在$M_2$中independent的当且仅当$X$在$M_1$中independent。记作$M_1\cong M_2$

如果一个拟阵与一个graph matroid 同构那么称为graphic 

如果一个拟阵$M$与vector matriod  of matrix D over filed F同构，那么称M为representable over F or F-representable，D称为representation for M over F or an F-represention for M

# 1.2

称**最大**的independent set为base of M

## 引理 1

如果$B_1,B_2$为$M$的基，那么$|B_1|=|B_2|$

证明：如果$|B_1|<|B_2|$，那么根据I3，可以知道存在$e\in B_2-B_1$，使得$B_1\cup e\in \mathbb{I}$，那么与$B_1$的最大性矛盾，因此$|B_1|\geq |B_2|$，同样的$|B_2|\geq |B_1|$



假设$\mathcal{B}$为所有M的基的族，满足以下两条性质$B1,B2$

- $\mathcal{B}$非空
- 假设$B_1,B_2\in \mathcal{B}$并且$x\in B_2-B_1$，那么一定存在$y\in B_2-B_1$满足$(B_1-x)\cup y\in \mathcal{B}$

第二条性质的证明就是利用I3的条件

可以证明满足这两个条件是判定$\mathcal{B}$为M基的族的充分必要条件



## 推论

令$B$为M的一个基，如果$e\in E(M)-B$，那么集合$B\cup e$包含一个唯一的circuit$C(e,B)$，并且$e\in C(e,B)$



对于一个graphic拟阵$M$，有$M\cong M(G),G\ is\ connected $

# 1.3 rank

令$M$为拟阵$(E,\mathcal{I})$，假设$X\subseteq E$，令$\mathcal{I}|X$为$\{I\subseteq X:I\subseteq\mathcal{I}\}$那么$(X,\mathcal{I}|X)$也是一个拟阵，称之为restriction of M to X or the deletion of E-X from M，记作$M|X$或者$M\backslash(E-X)$
$$
\varrho (M|X)=\{C\subseteq X:C\in \varrho (M)\}
$$
由于$M|X$的所有基的大小都是相同的，那么将这个基的大小记作$r(X)$，往往将$r(E(M))$记作$r(M)$，这个相当于作为一个函数将$2^E$个子集映射到非负整数

这个函数满足以下三条性质$R1,R2,R3$

- 对于$X\subseteq E$，那么$0\leq r(x)\leq |X|$
- 如果$X\subseteq Y\subseteq E$，那么$r(X)\leq r(Y)$

- 如果$X,Y\subseteq E$，那么有$r(X\cup Y)+r(X\cap Y)\leq r(X)+r(Y)$

关于第三条性质的证明，首先可以发现$B_{X\cup Y}\cap X,B_{X\cup Y}\cap Y$分别是independent in M|X and M|Y

因此有$|B_{X\cup Y}\cap X|\leq r(X),|B_{X\cup Y}\cap Y|\leq r(Y)$
$$
\begin{align}
r(X)+r(Y)&\geq|B_{X\cup Y}\cap X|+|B_{X\cup Y}\cap Y|\\
&=|(B_{X\cup Y}\cap X)\cup (B_{X\cup Y}\cap Y)|+|(B_{X\cup Y}\cap X)\cap (B_{X\cup Y}\cap Y)|\\
&=|B_{X\cup Y}\cap(X\cup Y)|+|B_{X\cup Y}\cap X\cap Y|
\end{align}
$$
由于$B_{X\cup Y}\cap(X\cup Y)=B_{X\cup Y},B_{X\cup Y}\cap X\cap Y=B_{X\cap Y}$

那么$r(X)+r(Y)\geq r(X\cup Y)+r(X\cap Y)$

可以证明通过这三个条件是可以唯一确定一个拟阵的r函数



对于$X\subseteq E(M)$

- 如果X是independent 当且仅当$|X|=r(X)$

- 如果X是M的基当且仅当$|X|=r(X)=r(M)$

- 如果X是circuit当且仅当X是非空的，并且$\forall x\in X$，有$r(X-x)=|X|-1=r(X)$

# 1.4 closure

定义函数$cl:2^E\rightarrow 2^E$
$$
cl(X)=\{x\in E:r(X\cup x)=r(X)\}
$$
如果用线性代数的角度看，也就是这个相当于将X能够线性表示出来的所有向量求出来

满足以下四条性质$CL1,CL2,CL3,CL4$

- 如果$X\subseteq E$，那么有$X\subseteq cl(X)$
- 如果$X\subseteq Y\subseteq E$，那么$cl(X)\subseteq cl(Y)$
- 如果$X\subseteq E$，那么$cl(cl(X))=cl(X)$
- 如果$X\subseteq E,x\in E$，并且$y\in cl(X\cup x)-cl(X)$，那么$x\in cl(X\cup y)$

前三条性质比较显然

## 引理 1

如果$X\subseteq E,x\in E$，那么有$r(X)\leq r(X\cup x)\leq r(X)+1$

证明就是要么是$B_X$为$X\cup x$的基要么$B_X\cup x$为$X\cup x$的基



那么对于$y\in cl(X\cup x)-cl(X)$，有$r(X\cup x\cup y)=r(X\cup x),r(X\cup y)= r(X)+1$
$$
r(X)+1=r(X\cup y)\leq r(X\cup y\cup x)=r(X\cup x)\leq r(X)+1
$$
那么$r(X\cup y)=r(X\cup y\cup x)$

那么根据定义显然有$x\in cl(X\cup y)$

## 定理 1

让$cl:2^E\rightarrow 2^E$满足$CL1,...,CL4$，那么有
$$
\mathcal{I}=\{X\subseteq E:x\notin cl(X-x),\forall x\in X\}
$$
可以这样理解，如果$x\in cl(X-x)$，那么说明x就可以被其他X中的元素线性表示出来，那么X中的元素就不是 线性无关的

证明这个定理需要一个引理

## 引理 2

假设$X\subseteq E,x\in E$，如果$X\in \mathcal{I}$，并且$X\cup x\notin \mathcal{I}$，那么$x\in cl(X)$

证明：由于$X\cup x\notin \mathcal{I}$，那么一定存在$y\in cl((X\cup x)-y)$，如果$y=x$，那么定理证明结束

那么$x\neq y$，那么$(X\cup x)-y=(X-y)\cup x$，那么就有$y\in cl((X-y)\cup x)-cl(X-y)$（因为$cl((X\cup x)-y)\subseteq cl(X-y)$），根据性质4，那么有$x\in cl((X-y)\cup y)=cl(X)$



给出几个定义

如果$X=cl(X)$，那么称$X$为a flat or a closed set of M

如果一个flat 的rank为$r(M)-1$，那么就称为hyperplane

一个$E(M)$的子集$X$称为spanning set 当且仅当$cl(X)=E(M)$

## 命题 1

$X$为spanning set 当且仅当$r(X)=r(M)$

证明：令$B$为$X$的一个基，那么对于所有的$x\in X-B$，都有$B\cup x\notin \mathcal{I}(M)$

那么根据引理 2，可以知道$x\in cl(B)$，那么可以得到$X\subseteq cl(B)$，那么$E=cl(X)\subseteq cl(B)\subseteq E$，所以$B$为$M$的一个基，那么就有$r(X)=|B|=r(M)$

## 命题 2

如果$X$是一个基当且仅当$X$是最小的spanning set

## 命题 3

X是hyperplane当且仅当X是最大的non-spanning set



关于cl与circuit的关系有如下两个事实

- X是circuit当且仅当$X$是满足$\forall x\in X,x\in cl(X-x)$的最小集合
- $cl(x)=X\cup \{x: \exist circuit\ C ,x\in C\subseteq X\cup x\}$

其中第一个是显然的

# 1.5

称一个可重集合$\{v_1,v_2,...,v_k\}$是affinely dependent，当且仅大存在不全是0的$a_1,a_2,...,a_k$，满足$\sum\limits_{i=1}^k a_iv_i=0,\sum\limits_{i=1}^k a_i=0$

这个判定等价于，可重集合$\{(1,v_1),(1,v_2),...,(1,v_k)\}$中的元素是线性相关的

如果$E$是一个元素为$V(m,F)$中的可重集合，令$\mathcal{I}$为$E$中的affinely independent的子集族，那么$(E,\mathcal{I})$就是一个拟阵

证明：假设$E=\{v_1,v_2,...,v_n\}$，那么可以构造一个$(m+1)\times n$的矩阵$A$，其中第$i$列都是$(1,v_i)^T$，那么$(E,\mathcal{I})=M[A]$

根据之前的推导，就可以知道这样可以构成一个拟阵

# 1.6 transversal matroid

令$S$为一个有限集合，a family of subsets of S是一个有限的序列$(A_1,A_2,...,A_m)$使得$j\in\{1,2,...,m\},A_j\subseteq S$，其中A不需要两两不同

如果$J=\{1,2,...,m\}$，那么把$(A_1,A_2,...,A_m)$记作$(A_j:j\in J)$

称集合$T$为transversal of $(A_j:j\in J)$如果存在一个双射$\psi:J\rightarrow T$使得$\psi(j)\in A_j,\forall j\in J$

如果$X\subseteq S$，称$X$为partial transversal of $(A_j:j\in J)$当有$K\subseteq J$，满足$X$是$(A_j:j\in K)$的transversal

可以构造如下的二分图变成一个匹配问题，左侧点集为$S$，右侧点集为$J$

边集为$\{x\rightarrow j:x\in S,j\in J,x\in A_j\}$

那么$(A_j:j\in J)$的transversal就要求右侧点全部匹配，一个partial transversal要求是一个匹配

## 定理 1

令$\mathcal{A}$为$(A_1,A_2,...,A_m)$，其中的$A_i\subseteq S$，令$\mathcal{I}$为$\mathcal{A}$中partial transversal的集合，那么$(S,\mathcal{I})$就是一个拟阵

证明：首先$I1,I2$是显然满足的

现在需要证明$I3$，假设$I_1,I_2\in \mathcal{I},|I_1|<|I_2|$，将其在二分图中的匹配边集记作$W_1,W_2$

那么现在给$W_1-W_2,W_2-W_1,W_1\cap W_2$中的边分别染上红色，蓝色，紫色，我们只考虑红色和蓝色导出的子图，显然其中不会出现紫色涉及到的点

由于红色的边比蓝色的边多，那么一定可以找到一条颜色交替出现的路径$v_1,v_2,...,v_{2k}$其中$v_1\in S,v_{2k}\in J$，使得开头和结尾的颜色都是蓝色，那么翻转这条路径上的颜色，得到的红色边就多了一条

那么$I_1\cup v_1\in \mathcal{I}$

# 1.7

称一个简单图$\tilde{G}$跟给定的$G$有关，是从G中删除所有自环，将重边替代为1条边得到的

如果一个元素是自环(loop of M)当且仅当$e$不存任何$M$的基中出现

如果元素$e_1$不是loop of M，并且$e_2$是平行于$e_1$的，那么对于一个包含$e_1$的基$B$，$(B-e_1)\cup e_2$还是一个基

根据这两条可推出，对于$x,y$两个不同的元素，并且是平行的，将$\mathcal{B_x}$记作所有包含$x$的基，那么$\mathcal{B_x}\cap \mathcal{B_y}=\empty,(B-x:B\in \mathcal{B_x})=(B-y:B\in \mathcal{B_y})$，说明只要在等价类中保留一个元素，就可以唯一确定这个拟阵，将利用这种方法简化过的拟阵称为$\tilde{M}$ simple matroid associated with M

另外一种看待$\tilde{M}$的方式就是，对于所有1-flat$\{Y_1,Y_2,...,Y_k\}$来说$r(Y_1\cup Y_2\cup ... \cup Y_k)=k$

考虑一个偏序集$(P,<)$，对于$x,y,z\in P$，满足$P1,P2,P3$

- $x\leq x$
- 如果$x\leq y,y\leq x$，那么$x=y$
- 如果$x\leq y,y\leq z$，那么$x\leq z$

如果$x<y$，并且不存在$x<z<y$，那么称y cover x

# 1.8

考虑最小生成树的问题，令$G$为一个联通图，假设有一个权值函数$w:E(G)\rightarrow \mathbb{R}$，对于$X\subseteq E(G)$，定义$w(X)=\sum\limits_{x\in X} w(x)$

令$\mathcal{I}$为$E$的子集组满足$I1,I2$，一个最优化问题记作$(\mathcal{I},w)$

贪心的过程如下

- 将$X_0=\empty,j=0$
- 如果$E-X_j$包含一个元素$e$使得$X_j\cup e\in \mathcal{I}$，选择其中权值最大的元素$e_{j+1}$，然后令$X_{j+1}=X_j\cup e_{j+1}$，然后到第三步，否则令$B_G=X_j$，退出过程
- 将j加1

## 引理 1

如果$(E,\mathcal{I})$为拟阵$M$，那么$B_G$就是这个最优化问题的解

证明：如果$r(M)=r$，那么$B_G=\{e_1,e_2,...,e_r\}$并且是M的基，假设$B=\{f_1,f_2,...,f_r\}$为$M$的另一个基，满足$w(f_1)\geq w(f_2)\geq ...\geq w(f_r)$

这个引理得证，如果引理2正确

## 引理 2

对于$1\leq j\leq r$，满足$w(e_j)\geq w(f_j)$

证明比较显然，利用反证法就可以了



## 定理 2

如果$(E,\mathcal{I})$为一个拟阵，当且仅当满足以下三个条件$I1,I2,G$

- $\empty\in \mathcal{I}$
- 如果$I\subseteq \mathcal{I},I'\subseteq I$，那么有$I'\in \mathcal{I}$
- 对于所有的权值函数$w:E\rightarrow \mathbb{R}$，贪心过程都是给出了$\mathcal{I}$中权值和最大的集合

证明：我们需要验证满足这三个条件的情况一定也满足$I3$

考虑反证法，假设$I_1,I_2\in \mathcal{I},|I_1|<|I_2|$，使得$\forall e\in I_2-I_1,I_1\cup e\notin \mathcal{I}$

由于$|I_1-I_2|<|I_2-I_1|,I_1-I_2\neq \empty$，那么我们可以选择一个整数$\epsilon$
$$
0< (1+\epsilon )|I_1-I_2|<|I_2-I_1|
$$
我们定义$w:E\rightarrow \mathbb{R}$
$$
w(e)=\left\{
\begin{aligned}
&2,\ e\in I_1\cap I_2\\ 
&\frac{1}{|I_1-I_2|},\ e\in I_1-I_2\\
&\frac{1+\epsilon}{|I_2-I_1|},\ e\in I_2-I_1\\
&0,\ otherwise
\end{aligned}
\right.
$$
这个会让贪心算法优先选择$I_1\cap I_2$中的边，然后$I_1-I_2$中的边

根据假设无法选出$I_2-I_1$中的元素，那么$B_G$剩下的元素，就是在$E-(I_1\cup I_2)$，那么$w(B_G)=2|I_1\cap I_2|+1$

但是由于$I2$，那么$w(I_2')\geq w(I_2)=2|I_1\cap I_2|+1+\epsilon$

那么导出矛盾
# 57.分形

一般来说分形都是具有很强的递归性，如果使用递归算法可以很清楚表示这个分形

最基本的思路应该是将图形分成若干块，然后一个高级块，是由若干低一级的块加上一些线组成的

可能根据这个思路，写出一般的递归式

对于封闭图形需要注意的是可能会有常数级别的线不满足关系，只需要按照这些线划分图形，最后将所有块和这些线拼接起来就行了

而这些图形往往是某一个图形通过旋转得到的，具有很强的对称性

还需要注意一下线段的长度问题，如果确定最低一级的单位长度，那么画出的每一条线段都是等长的

但如果需要保留比例，那么每一级的长度就需要计算，分别对长宽除以对应的线段数量

# UVA177

## 题目大意

给一个纸，按照一个方向，折$n$次，然后将每一个折痕，从180度翻折，变成90度翻转，求从底部看这个纸的形状
$$
n\leq 13
$$

## 算法讨论

通过模拟一下过程，可以发现，下一个图形是上一个图形顺时针旋转90度，然后拼接上原来图形得到的，那么就可以将每一个边看作一个向量，有4个方向依次是$RULD$，然后假设原点是(0,0)，每一个就是将向量旋转90度，最终得到这个总的向量数组，然后就按这个向量不断走，走出的范围可以用矩形表示，最大数据下这个矩形边长不会超过$250$​，一行一行扫描这个矩形就可以输出结果了

正规递归做法

设$f(i,op,d)$表示绘制出第$i$级图形，当前是否翻转$op$，旋转的角度为$d$​，按照上面得出的规律，可以写出以下的递归式
$$
f(i,0,d)\leftarrow f(i-1,0,d),f(i-1,1,d+90)\\
f(i,1,d)\leftarrow f(i-1,0,d+90),f(i-1,1,d)
$$
如果最后是第一级图形，那么之间按照旋转的角度输出一条线段就可以了

这里不存在线段长度的问题，每一条线段都是等长的，输出就可以了

由于输出格式的问题，需要存入矩阵中，一层一层扫描才行

如果用python turtle画出来，就可以直接实现这个$f$​

![python turtle](D:\Blog\image\python turtle.PNG)

第一次提交:AC

# UVA1607

## 题目大意

给定n个输入，m个NAND，输入如果是11，那么就输出0，否则输出1，求最少保留几个变量
$$
n,m\leq 1e5
$$

## 算法讨论

可以发现，最终只保留一个变量肯定最优，因为保留两个变量，一定会有一个位置这个两个变量相遇，由于NAND的性质，可以发现，其中一个变成常数，不会影响所有x的取值结果

那么如果一开始所有为0和所有为1的输出是相同的，那么跟x无关

否则由于在某一个位置与x位置取值有关，那么一旦改变这个位置，输出结果就会改变，那么可以通过二分这个位置求解

时间复杂度$O(n\log n)$

第一次提交:AC
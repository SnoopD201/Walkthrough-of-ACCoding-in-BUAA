# C2-2018级算法第二次上机

# 妙妙趣和值

时间限制: 1000 ms 内存限制: 65536 kb

总通过人数: 48 总提交人数: 56

`超难题，先看别的`

`题面里带Zexal的都不会太难～`

## 题面

定义一个长度为k的数组a1,a2,⋯,ak的妙妙趣值是min1≤i<j≤k |ai−aj|

那么请问对于一个长度为n的`序列`，所有长度为k的`子序列`的妙妙趣值和为多少。

某个序列的`子序列`是从最初序列通过去除某些元素但不破坏余下元素的相对位置（在前或在后）而形成的新序列。

答案为`100000007`取模

## 输入

第一行两个数n,k，表示序列的长度，子序列的长度。(2≤k≤n≤1000)

接下来一行，n个整数a1,a2,⋯,an,(0≤ai≤10^5)

## 输出

一行一个数，表示妙妙趣值和

## 输入样例

```
4 3
1 7 3 5
```

## 输出样例

```
8
```

## 样例解释

长度为3的子序列有 [1,7,3], [1,3,5], [7,3,5], [1,7,5]，每一个妙妙趣值都是2

## 思路

**这是一个思维量比较大的dp问题**

首先可以直接排序（**不难看出排序前后对结果是毫无影响的**）

 **然后考虑非下降序列当中,dp(x) = 长度k x<= |ai - aj|的子序列个数**

 **dp(i,j)是前i个里,长度j x<=|ai - aj|的序列个数**

 **所以dp(i,j)=dp(i-1,j)+dp(Li,j-1) Li是最靠后满足aLi+x<=a[i]的最大值**

 **min|ai-aj|=x则是dp(x)-dp(x+1)，dp(x)为x<=min|ai-aj|的子序列个数**

 **总和中需要加入x\*(dp(x)-dp(x+1))，而很显然通过前缀和,直接把dp(x)加进去就是对的了**

 **而dp(x)就是dp(n, k)**

 

 接下来需要注意x的上界,如果枚举到10^5显然会超时...

 **可以证明,实际上最大不会超过(max-min)/(k-1)的**

 所以只需要做这么多次dp就可以了

# 与非门

时间限制: 1000 ms 内存限制: 65536 kb

总通过人数: 33 总提交人数: 40

`难题，先看别的`

## 题面

数字电路中有一种基本逻辑电路叫做`与非门`，它有两个输入和一个输出。如下图：

![与非门](http://tva1.sinaimg.cn/large/007X8olVly1g7xu1qg1vpj30h303xjrd.jpg)

现在将n个与非门拼接到一起，形成了一个形如二叉树的电路，如下图：

![与非门](http://tva1.sinaimg.cn/large/007X8olVly1g7xu1y642oj309u04pmx1.jpg)

两个与非门相连表示一个与非门的输出作为另一个与非门的输入。不与与非门相连的部分表示外部输入，可能是0或者1。也就是说所有子节点数不为2的节点都会连一个外部输入，自底向上处理，最后从根节点输出，保证根节点编号为1.

由于外界的干扰，有一些与非门损坏，只能输出1或者只能输出0。

现在给出一个电路，请问有多少输入对应的输出会出错。

## 输入

第一行一个整数n（1≤n≤10^5）

接下来n行，每行三个整数a,b,c(0≤a,b≤n,−1≤c≤1)。对于第i行，如果a,b不为0，则a表示第i个与非门与第a个与非门相连，b表示第i个与非门与第b个与非门相连；如果a或者b为0，表示第i个与非门对应的输入接外部输入。c如果等于-1，表示第i个与非门正常工作，1表示第i个与非门只输出1，0表示第i个与非门只输出0

从1开始编号，保证根节点为1。

## 输出

输出一行，一个整数，答案可能很大，请对10^9+7取模

## 输入样例

```
4
2 3 1
0 0 -1
4 0 0
0 0 -1
```

## 输出样例

```
15
```

## 样例说明

这个样例对应的电路如下:

![与非门](http://tva1.sinaimg.cn/large/007X8olVly1g7xu1y642oj309u04pmx1.jpg)

其中1号损坏只能输出1,3号损坏只能输出0

一共有5个外部输入，每个都可能是0,1,所以输入一共有2^5种情况

对于输入`0 0 0 0 0`（上图中，由上到下），理论输出为0，实际输出为1，出错

对于输入`1 1 1 1 1`（上图中，由上到下），理论输出为1，实际输出为1，正确

## 思路

**这题可以看出是一个二叉树的树形结构，应该能看出是树形dp的一个基本题目，关键在求出递推方程**

毕竟外部节点有n+1个，总共的可能情况是2^(n+1) 你总不能直接枚举2^(n+1)种情况吧...

**这题对树形结构不太熟悉的话，首先需要补一下树形问题怎么做dp，怎么dfs...**

那么我们首先可以看到关于与非门的各种运算是这样的，然后有坏掉的门

**所以我们可以先从根开始dfs，最先进行运算的都是和2个外部节点相接的叶节点开始计算**

**我们设dp(i, j, k)表示的是第i个节点 理论上是j 实际上是k的方案个数，那么只要j != k实际上就错了**

**那么实际上我们可以枚举他的左右子节点的各种可能性，一共是16种**

**那么我们可以知道dp(i, j, k) = ∑(dp(lchild(i), a, b) \*  dp(rchild(i), c, d))    **

**其中a, b, c, d = 0, 1  j是a和c的与非值  如果电路i没坏的话 k是b和d的与非值,否则就是自己固定的逻辑值**

然后每个子节点这样计算完，就可以回溯到父节点进行计算

**直到回溯至根节点，那么dp(root, 1, 0) + dp(root, 0, 1)即为所求，本题明确root = 1，如果没有明确，则需要通过记录每个点的入度，入度为0的即为根**

# 竞赛

时间限制: 1000 ms 内存限制: 65536 kb

总通过人数: 216 总提交人数: 238

## 题目描述

在一场竞赛中有𝑛道题目，每道题都会对应一个分值，你可以选择任意一道题作答，比如你选择了𝑥分的题目，那么在作答完毕（假设一定可以得分）后，你将获得x分，但是这场比赛中分值**等于**𝑥−1和𝑥+1的其他题目就会消失，那么这场比赛中你最多可以得到多少分？

## 输入

第一个数为题目总数𝑛（0<𝑛<1𝑒5)

接下来为𝑛个n个整数𝑎1，𝑎2，𝑎3.....(0<𝑎𝑖<1𝑒5)

## 输出

输出你可以得到的最高分

## 输入样例

```
9
1 2 1 3 2 2 2 2 3
```

## 输出样例

```
10
```

## 样例解释

```
每次都选择2 选择5次即可得到10分 1和3根据题目要求会消失
```

## 思路

**本题是较为基础的动态规划问题**

**设dp[n]是直到n这里可以取到的最大分数值，所以要么不取自己分值的题，要么取了自己不能取n-1**

**所以很显然有dp[n] = max(dp[n-1], a[n] + dp[n-2])**

**然后最基础的有dp[1] = a[1], dp[2] = max(a[1], a[2])**

**基础的，a[i] = i\*cnt, cnt是分值为i的题目出现的次数**

# Zexal叒排座位

时间限制: 1000 ms 内存限制: 65536 kb

总通过人数: 112 总提交人数: 117

## 题目描述

一个班级里面有若干个小组，每个小组的人数不同，假设将两个小组的人的座位合并排列成一个小组的座位所需要的精力值为两个小组的人数之和，由于排座位的只有Zexal一个人，现在他可以一次合并三个小组，已知有N个小组，第i个小组的人数为a[i]。当小组数小于三的时候，排座位完成。求解这个过程所需要的最小精力值为？

## 输入

多组数据输入，第一个数为小组的数量𝑁N。（N<=1e6）

接下来N个整数，代表𝑁N个小组的人数。（在int范围内并用空格隔开）

## 输出

对于每组数据，输出一行，为所需要消耗的最小精值。（保证结果在int范围内）

## 输入样例

```
4
1 2 3 4
```

## 输出样例

```
6
```

## 思路

**18级C2的弃用题目**

根据题目给出的思路，**最后只剩下4和6的时候不再需要合并了，把上面的代码改改就行，助教并没有为难大家**

**然而上面这个题和2叉huffman树不同，并不是一个真正的3叉huffman树，如果需要把所有的元素都取完得到最小值的话，首先假设是n个元素和m个huffman树，那么当(n-1)无法整除(m-1)的时候需要补充对应个数的0，使其整除**

**换句话说，如果这题改成了真正的3叉huffman，上述4个元素都要取出来的话，则需要先额外push一个0进去，再做模拟**


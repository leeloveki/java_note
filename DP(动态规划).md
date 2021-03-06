# DP(动态规划)

> 从算法的优化角度

暴力递归遍历 -> 记忆化搜索 -> 动态规划

> 也可以认为是从个例情况的遍历到对一个集合(多个例)的枚举

在用动态规划解决问题前, 可以先写一个暴力递归的解决版本, 然后将暴力递归进一步优化为动态规划

动态规划以解决背包问题而闻名

动态规划是递归(函数调用自身)的一种特殊优化, 可以用于解决递归问题

动态规划可以理解为将问题划分为数个子问题, 然后通过递归将子问题的结果存储起来

**如何确定一个问题可以用动态规划解决**

一般DP的问题都可以用粗暴遍历所有的情况来解决, 但是这样做会导致时间复杂度和空间复杂度非常高

> 状态亦可以被称为最优解

因此DP从一个已知的状态出发, 根据会出现的情况计算出下一个状态, 直到达到目标状态

动态规划使用的前提是无后效性: 状态的唯一性, 不受前路径和后路径的影响

动态规划解决问题的关键是定义状态, 找到已有的状态以及不同状态间的转移方程

转移方程: 用于将通过计算将一个状态转换为另一条状态, 

动态规划一般有两种模式:  表模式	备忘录模式

表模式: 从下到上, 从底部开始计算状态, 从最底部的状态dp[0]开始通过状态转移方程逐渐转移到目标状态dp[n]

> 表模式中状态的转移是顺序的

示例

```java
// Tabulated version to find factorial x.
int dp[MAXN];

// base case
int dp[0] = 1;
for (int i = 1; i< =n; i++)
{
    dp[i] = dp[i-1] * i;
}
```

备忘录(记忆)模式: 从上到下

> 记忆模式中, 状态可以是非顺序转换的
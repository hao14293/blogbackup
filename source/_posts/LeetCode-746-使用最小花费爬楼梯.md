---
title: LeetCode 746. 使用最小花费爬楼梯(Python)
tags: LeetCode
categories: Algorithm and Data Structur
abbrlink: 54393
date: 2018-08-04 10:39:13
---
>  746.使用最小花费爬楼梯

<pre>
数组的每个索引做为一个阶梯，第 i个阶梯对应着一个非负数的体力花费值 cost[i](索引从0开始)。

每当你爬上一个阶梯你都要花费对应的体力花费值，然后你可以选择继续爬一个阶梯或者爬两个阶梯。

您需要找到达到楼层顶部的最低花费。在开始时，你可以选择从索引为 0 或 1 的元素作为初始阶梯。

示例 1:

输入: cost = [10, 15, 20]
输出: 15
解释: 最低花费是从cost[1]开始，然后走两步即可到阶梯顶，一共花费15。
 示例 2:

输入: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
输出: 6
解释: 最低花费方式是从cost[0]开始，逐个经过那些1，跳过cost[3]，一共花费6。
注意：

cost 的长度将会在 [2, 1000]。
每一个 cost[i] 将会是一个Integer类型，范围为 [0, 999]。
</pre>

<!--more-->
##### 思路：动态规划
<p style="font-size: 16px; font-weight: bold">求到达每一阶的最小成本。倒数第一和倒数第二的最小值即为解。 <br>
我是这么考虑的：把问题缩小，只有两种办法到达第i阶，一种是i-2阶走两步到达，一种是i-1阶走一步到达。<br>
因此到达台阶i的花费即为两种方法中代价最小的。表示为：cost[i] = min(cost[i-2]+cost[i],cost[i-1]+cost[i]). <br>
动态规划核心就是找到最优子结构，然后自上而下或者自底向上求解问题。如果对时间复杂度有要求的话，最好选择递推，相对递归来说效率高。</p>

<b>两种算法，一种通俗易懂，一种比较秀，原理一样。</b>
```sh
class Solution(object):
    def minCostClimbingStairs(self, cost):
        """
        :type cost: List[int]
        :rtype: int
        """
        dp = {}
        dp[0] = cost[0]
        dp[1] = cost[1]
        for i in range(2,len(cost)):
            dp[i] = min(dp[i-2]+cost[i],dp[i-1]+cost[i])
        return min(dp[len(cost)-1],dp[len(cost)-2])
            ```

```sh
class Solution(object):
    def minCostClimbingStairs(self, cost):
        """
        :type cost: List[int]
        :rtype: int
        """
        dp = [0] * 3
        for i in reversed(xrange(len(cost))):
            dp[i%3] = cost[i] + min(dp[(i+1)%3], dp[(i+2)%3])
        return min(dp[0], dp[1])
    ```
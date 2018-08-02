---
title: LeetCode 458. 可怜的小猪
tags: LeetCode
categories: Algorithm and Data Structure
abbrlink: 48466
date: 2018-07-25 14:41:14
---
> 很有意思的一个算法题

<pre>
题目：

有1000只水桶，其中有且只有一桶装的含有毒药，其余装的都是水。它们从外观看起来都一样。如果小猪喝了毒药，它会在15分钟内死去。

问题来了，如果需要你在一小时内，弄清楚哪只水桶含有毒药，你最少需要多少只猪？

回答这个问题，并为下列的进阶问题编写一个通用算法。

进阶:

假设有 n 只水桶，猪饮水中毒后会在 m 分钟内死亡，你需要多少猪（x）就能在 p 分钟内找出“有毒”水桶？n只水桶里有且仅有一只有毒的桶。
</pre>
<!--more-->



<b>  解题思路</b><br><br>

这道题最重要的就是解题思路。<br> 
 
其实最关键的就是用一个<b>n维空间定义坐标</b>的思想。<br> 
<b>注意，题目中并没有说1头小猪15分钟内只能喝1桶水。</b><br>

用题目中给的数据为例： <br>
每只猪在实验时间内可检测的数量是60/15+1=5（<b>需要注意的是最后要+1，因为如果前面的水喝完都没死的话，说明毒水肯定是最后一桶，也不用再喝了</b>）。 <br>
如果用两头小猪进行实验，则可将水桶定义为二维坐标，两头猪分别检测x与y方向的水：每头猪15分钟内需尝试5桶水（一排或一列）。 <br>
以此类推，3头猪则可推广到3维坐标，对应的，每头小猪每15分钟内尝试5^2桶水。 <br>
这样，每增加1头小猪，可检测的水桶数就乘5，问题解决效率就很高了。 <br>
理清了思路，代码其实很简单。

<pre>
class Solution(object):
    def poorPigs(self, buckets, minutesToDie, minutesToTest):
        """
        :type buckets: int
        :type minutesToDie: int
        :type minutesToTest: int
        :rtype: int
        """
        times = minutesToTest / minutesToDie + 1    #每头猪最多可测试的水桶数
        num = 0
        while pow(times,num) < buckets:
            num = num + 1
        return num
 </pre>
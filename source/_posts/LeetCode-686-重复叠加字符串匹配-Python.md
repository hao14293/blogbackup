---
title: LeetCode-686. 重复叠加字符串匹配(Python)
tags: LeetCode
categories: Algorithm and Data Structure
abbrlink: 47574
date: 2018-07-31 13:07:42
---
>  686.重复叠加字符串匹配

<pre>
给定两个字符串 A 和 B, 寻找重复叠加字符串A的最小次数，使得字符串B成为叠加后的字符串A的子串，如果不存在则返回 -1。

举个例子，A = "abcd"，B = "cdabcdab"。

答案为 3， 因为 A 重复叠加三遍后为 “abcdabcdabcd”，此时 B 是其子串；A 重复叠加两遍后为"abcdabcd"，B 并不是其子串。

注意:

 A 与 B 字符串的长度在1和10000区间范围内。
 </pre>
 
 <!--more-->
 ```sh
 class Solution(object):
    def repeatedStringMatch(self, A, B):
        """
        :type A: str
        :type B: str
        :rtype: int
        """
        if(A.count(B)>=1):
            return 1
        if(set(A)!=set(B)):
            return -1
        i=int(len(B)/len(A))
        while(1):
            C=A*i
            if(C.count(B)>0):
                return i
            elif i>3:
                return -1
            i+=1
            ```
  不明白的可以留言


---
title: LeetCode 720. 词典中最长的单词(Python)
tags: LeetCode
categories: Algorithm and Data Structure
abbrlink: 40533
date: 2018-08-04 00:41:58
---
> LeetCode 720. 词典中最长的单词

<pre>
给出一个字符串数组words组成的一本英语词典。从中找出最长的一个单词，该单词是由words词典中其他单词逐步添加一个字母组成。若其中有多个可行的答案，则返回答案中字典序最小的单词。

若无答案，则返回空字符串。

示例 1:

输入: 
words = ["w","wo","wor","worl", "world"]
输出: "world"
解释: 
单词"world"可由"w", "wo", "wor", 和 "worl"添加一个字母组成。
示例 2:

输入: 
words = ["a", "banana", "app", "appl", "ap", "apply", "apple"]
输出: "apple"
解释: 
"apply"和"apple"都能由词典中的单词组成。但是"apple"得字典序小于"apply"。
注意:

所有输入的字符串都只包含小写字母。
words数组长度范围为[1,1000]。
words[i]的长度范围为[1,30]。
</pre>

<!--more-->
先来个最简单，最暴力的解法<br>
##### 方法一
巧妙用了set（）,判断每个单词的除去倒数第一个字母是否在set里，用一个变量保存最长的单词。

用判断新单词是否比最长单词更长的方式完成两个需求：1.找出最长；2.同样最长的情况下，保留字母序最小的。这样做的前提是先对words进行排序。

```sh
class Solution(object):
    def longestWord(self, words):
        """
        :type words: List[str]
        :rtype: str
        """
        words.sort()
        res = set([''])
        longestWord = ''
        for word in words:
            if word[:-1] in res:
                res.add(word)
                if len(word) > len(longestWord):
                    longestWord = word
        return longestWord
```
    
    
    
> 这个<code>set</code>用的妙，这种方法学习一下。

<hr>

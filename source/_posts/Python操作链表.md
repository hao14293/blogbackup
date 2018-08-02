---
title: Python操作链表
tags: 算法
categories: Python
abbrlink: 17560
date: 2018-07-08 09:03:02
---
> 终于考完试了，又可以每天开心地敲代码了。
* * * * 


利用暑假时间，准备用Python把 LeetCode 刷一遍。今天做一道简单链表题的时候，突然发现用Python无法直接操作链表，在网上找了一下，发现可以自己定义实现，所以就打个笔记。<br>
<!--more-->
#### 1.定义链表

```sh
# Definition for singly-linked list.
class ListNode(object):
	def __init__(self):
    	self.val = None
        self.next = None
     ```
#### 2.对链表的操作

```sh
class ListNode_handle:
	def __init__(self):
    	self.cur_node = None
        
    def add(self, data):
    	# add a new node pointed to previous node
        node = ListNode()
        node.val = data
        node.next = self.cur_node
        self.cur_node = node
        return node
        
    def print_ListNode(self, node):
    	while node:
        	print '\nnode: ', node, ' value: ', node.val, ' next: ', node.next
            node = node.next
            
    def _reverse(self, nodelist):
    	list = []
        while nodelist:
        	list.append(nodelist.val)
            nodelist = nodelist.next
        result = ListNode()
        result_handle = ListNode_handle()
        for i in list:
        	result = result_handle.add(i)
        return result
```
如 要将1,8,3按照1-->8-->3的顺序使用ListNode_1操作放入链表ListNode()中，可以进行如下操作
```sh
ListNode_1 = ListNode_handle()
l1 = ListNode()
l1_list = [1,8,3]
for i in l1_list:
	l1 = ListNode_1.add(i)
```
反向排列列表
```sh
l1 = ListNode_1.reverse(l1)
```
打印链表
```sh
ListNode_1.print_ListNode(l1)
```
##### 如这个算法题
<pre>
给定两个非空链表来表示两个非负整数。位数按照逆序方式存储，它们的每个节点只存储单个数字。将两数相加返回一个新的链表。

你可以假设除了数字 0 之外，这两个数字都不会以零开头。

示例：

输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807

</pre>

```sh
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        if l1 is None:
            return l2
        if l2 is None:
            return l1
 
        tmp = ListNode(0)
        res = tmp
        flag = 0
        while l1 or l2:
            tmpsum = 0
            if l1:
                tmpsum = l1.val
                l1 = l1.next
            if l2:
                tmpsum += l2.val
                l2 = l2.next
            tmpres = ((tmpsum + flag) % 10)
            flag = ((tmpsum + flag) // 10)
            res.next = ListNode(tmpres)
            res = res.next
        if flag:
            res.next = ListNode(1)
        res = tmp.next
        del tmp
        return res
            ```
---
layout:     post
title:      "数据结构最优解之链表Lab2.5"
subtitle:   "链表的部分反转"
date:       2017-04-13 14:28:46
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “非连续非顺序、动态地进行存储分配的存储结构“

# 前言

是“反转链表”的进阶题目哦，虽说也不难就是了。

---

# 正文

**题目：在链表中反转部分链表**

要求是：给定单向链表头结点head，以及两个整数from、to，在单向链表上把第from个到第to个节点进行反转。假设链表长度为N，时间复杂度为O(N)，额外空间复杂度为O(1)。

**思路分析**
 
 - 如果不符合1<=from<=to<=N，不用调整。
 - 确认第from-1节点fpre和第to+1的节点tpos，fpre是反转部分的前一个节点，tpos是反转部分的后一个节点。
 - 反转要反转的部分，然后正确连接fpre和tpos。
 - 如果fpre为null则反转部分包含头节点。
 
**代码实现**

***链表节点***

```
class Node{
	public int value;
	public Node next;
	
	public Node(int data) {
		this.value = data;
	}
}
```
  
***过程实现***

```
public class Lab2_5 {
	public Node reversePart(Node head, int from, int to) {
		int n = 0;
		Node cur = head, fnode = null, tnode = null;
		// 链表长度n，from的前一个节点fnode，to的后一个节点tnode
		while (cur != null) {
			n++;
			if (n == from - 1) {
				fnode = cur;
			}
			if (n == to + 1) {
				tnode = cur;
			}
			cur = cur.next;
		}
		if (from > to || from < 1 || to > n) {
			return head;
		}
		if (fnode == null) {
			cur = head;
		} else {
			cur = fnode.next;
		}
		Node cur2 = cur.next;
		// from节点连接tnode
		cur.next = tnode;
		Node next = null;
		while (cur2 != tnode) {
			next = cur2.next;
			cur2.next = cur;
			cur = cur2;
			cur2 = next;

		}
		if (fnode != null) {
			fnode.next = cur;
			return head;
		}
		return cur;
	}
}
```  
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%BA%8C%E7%AB%A0)

---
layout:     post
title:      "数据结构最优解之链表Lab2.4"
subtitle:   "反转链表"
date:       2017-04-06 13:48:36
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “非连续非顺序、动态地进行存储分配的存储结构“

---

# 正文

**题目：反转链表，无论单向还是双向**

要求是：假设链表长度为N，时间复杂度为O(N)，额外空间复杂度为O(1)。

**思路分析**
 
 - 关键点在于对每一个节点，破坏其与后一个节点的连接同时新建其与上一个节点的连接。
 - 新建node用以存放上一个节点。
 
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

class DoubleNode {
	public int value;
	public DoubleNode next;
	public DoubleNode last;
	
	public DoubleNode(int data) {
		this.value = data;
	}
}
```
  
***过程实现***

```
public class Lab2_4 {
	//单链表
	public Node reverseList(Node head) {
		if (head.next == null) {
			return head;
		}
		Node next = null, pre = null;
		while (head != null) {
			next = head.next;
			head.next = pre;
			pre = head;
			head = next;
		}
		return pre;
	}

	//双链表
	public DoubleNode reverseLists(DoubleNode head) {
		if (head.next == null) {
			return head;
		}
		DoubleNode pre = null, next = null;
		while (head != null) {
			next = head.next;
			head.next = pre;
			head.last = next;
			pre = head;
			head = next;
		}
		return pre;
	}
}

```  
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%BA%8C%E7%AB%A0)

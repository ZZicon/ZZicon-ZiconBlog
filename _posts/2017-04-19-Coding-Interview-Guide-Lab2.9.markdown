---
layout:     post
title:      "数据结构最优解之链表Lab2.9"
subtitle:   "链表的复制"
date:       2017-04-19 19:22:46
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “非连续非顺序、动态地进行存储分配的存储结构“

---

# 正文

**题目：复制含有随机指针节点的链表**

要求是：给定一个无环单链表头结点head，实现复制该链表中所有结构包含随机指针。

**思路分析** 
 
 - 第一次遍历链表，复制的新的节点放在被复制节点与下一个节点之间。
 - 第二次遍历设置每一个复制节点的rand节点。
 - 将复制的节点提取形成链表并返回。

**代码实现**

***链表节点***

```
class Node{
	public int value;
	public Node next;
	public Node rand;
	
	public Node(int data) {
		this.value = data;
	}
}
```
  
***过程实现***

```
public class Lab2_9 {
	public Node copyListNode(Node head) {
		if (head == null || head.next == null) {
			return head;
		}
		Node cur = head;
		Node next = null;
		while (cur != null) {
			next = cur.next;
			cur.next = new Node(cur.value);
			cur.next.next = next;
			cur = next;
		}

		cur = head;
		Node copy = null;
		while (cur != null) {
			next = cur.next.next;
			copy = cur.next;
			if (cur.random.next == null) {
				copy.random = null;
			} else {
				copy.random = cur.random.next;
			}
		}

		// 分离
		cur = head;
		Node res = head.next;
		while (cur != null) {
			next = cur.next.next;
			copy = cur.next;
			cur.next = next;
			copy.next = next != null ? next.next : null;
			cur = next;
		}
		return res;
	}
}
```  
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%BA%8C%E7%AB%A0)

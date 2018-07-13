---
layout:     post
title:      "数据结构最优解之链表Lab2.3"
subtitle:   "链表删除中间节点"
date:       2017-04-06 13:08:16
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “非连续非顺序、动态地进行存储分配的存储结构“

---

# 正文

**题目：在链表中删除中间节点**

要求是：给定链表的头结点head，实现删除链表的中间节点。

**思路分析**
 
 - 链表为空或长度为1时不需要调整，直接返回。
 - 链表长度每增加2（3、5、7），中间节点的位置就后移一位（2、3、4）。
 
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
public Node removeMidNode(Node head) {
		if (head == null || head.next == null) {
			return head;
		}
		if (head.next.next == null) {
			return head.next;
		}
		Node pre = head;
		Node cur = head.next.next;
		while (cur.next != null && cur.next.next != null) {
			pre = pre.next;
			cur = cur.next.next;
		}
		pre.next = pre.next.next;
		return head;
	}
```  

这道题目很简单，只要了解到链表长度与链表中点的联系就可以轻松的解答。但是，如果题目要求的不再是中间而是任意节点呢？甚至是一个模糊的几分之几的位置的节点呢？

**题目进阶：删除链表的a/b位置的节点**

要求是：给定链表的头结点head，整数a、b，实现删除链表的a/b处的节点。

**思路分析**
 
 - 链表为空或长度为1时不需要调整，直接返回。
 - 首先需要确认a/b处的节点，即该节点是a/b * n（链表长度）后向上取整的值。
 - 记为Math.ceil(((double) (a * n)) / (double) b );

***过程实现***

``` 
public Node removeNumNode(Node head, int a, int b) {
		if (a < 1 || a > b) {
			return head;
		}
		int n = 0;
		Node cur = head;
		// 获得链表长度
		while (cur != null) {
			n++;
			cur = cur.next;
		}
		n = (int) Math.ceil(((double) (a * n)) / (double) b);
		if (n == 1) {
			head = head.next;
		}
		if (n > 1) {
			cur = head;
			// 寻找要删除节点的前一个节点
			while (--n != 1) {
				cur = cur.next;
			}
			cur.next = cur.next.next;
		}
		return head;
	}
``` 

# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%BA%8C%E7%AB%A0)

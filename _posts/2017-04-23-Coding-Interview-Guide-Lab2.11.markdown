---
layout:     post
title:      "数据结构最优解之链表Lab2.11"
subtitle:   "链表相交"
date:       2017-04-23 16:29:46
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “非连续非顺序、动态地进行存储分配的存储结构“

---

# 正文

**题目：两个单链表相交的问题**

要求是：给定两个单链表头节点head1、head2，链表可能有环，判断两个链表是否相交。假设两个链表的长度分别为N、M，要求时间复杂度为O(N+M)，额外空间复杂度为O(1)。

**思路分析** 
 
仔细分析题目，可以分为三个部分。

 - 问题一，判断一个链表是否有环，有环则返回第一个入环的节点，没有则返回null。
 - 问题二，判断两个无环链表是否相交，相交则返回第一个相交节点，不相交返回null。
 - 问题三，判断两个有环链表是否相交，相交则返回第一个相交节点，不相交返回null。
 - 注意，一个链表有环而另一个链表无环，它们是不可能相交的。

***问题一***

 - 判断有无环，使用追赶的方法，设定两个指针slow、fast，从头指针开始，每次分别前进1步、2步。如存在环，则两者相遇；如不存在环，fast遇到NULL退出。。
 - 返回入环节点，相遇后fast指针回到head位置，slow指针不变，两者都只移动一步，当其再次相遇时就是第一个入环节点。
 
当通过问题一得知两个链表是否有环后就可以进入下一阶段，针对链表是否有环分别进入问题二或问题三的解法。
 
***问题二***

 - 分别遍历两个链表，记录其长度len1、len2与最后一个节点end1、end2。
 - 如果end1等于end2，两个链表相交；反之不相交返回null。
 - 记录len1-len2的绝对值len，让较长的链表先遍历len节点，之后两个链表同时向后遍历，当他们相等时就是第一个相交节点。

***问题三***

 - 对于两个有环链表，比较其第一个入环节点，如果两个链表的第一个入环节点相等。说明他们的第一个相交节点在链表头结点与入环节点之间，求法可参考问题二。
 - 入环节点不相等的情况下，让链表1从其入环节点开始往后遍历，如果遍历时遇到链表2的入环节点说明两个链表相交，此时返回两个链表中任意的第一个入环节点均可；反之两个链表不相交，返回null。
 
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
public class Lab2_11 {

	// 无环链表是否相交
	public Node noLoop(Node head1, Node head2) {
		if (head1 == null || head2 == null) {
			return null;
		}
		int s = 0;
		Node cur1 = head1, cur2 = head2;
		while (cur1.next != null) {
			s++;
			cur1 = cur1.next;
		}
		while (cur2.next != null) {
			s--;
			cur2 = cur2.next;
		}
		if (cur1 != cur2) {
			return null;
		}
		if (s > 0) {
			cur1 = head1;
			cur2 = head2;
		} else {
			cur1 = head2;
			cur2 = head1;
		}
		s = Math.abs(s);
		while (s != 0) {
			s--;
			cur1 = cur1.next;
		}
		while (cur1 != cur2) {
			cur1 = cur1.next;
			cur2 = cur2.next;
		}
		return cur1;
	}

	// 返回成环链表的入环节点
	public Node getLoopNode(Node head) {
		if (head == null || head.next == null || head.next.next == null) {
			return null;
		}
		Node node1 = head.next;
		Node node2 = head.next.next;
		while (node1 != node2) {
			if (node2.next == null || node2.next.next == null) {
				return null;
			}
			node2 = node2.next.next;
			node1 = node1.next;
		}
		node2 = head;
		while (node1 != node2) {
			node1 = node1.next;
			node2 = node2.next;
		}
		return node1;
	}

	// 有环链表是否相交
	public Node bothLoop(Node head1, Node head2) {
		Node loop1 = getLoopNode(head1);
		Node loop2 = getLoopNode(head2);
		Node cur1 = null, cur2 = null;
		if (loop1 == loop2) {
			cur1 = head1;
			cur2 = head2;
			int n = 0;
			while (cur1 != loop1) {
				n++;
				cur1 = cur1.next;
			}
			while (cur2 != loop2) {
				n--;
				cur2 = cur2.next;
			}
			cur1 = n > 0 ? head1 : head2;
			cur2 = cur1 == head1 ? head2 : head1;
			n = Math.abs(n);
			while (n != 0) {
				n--;
				cur1 = cur1.next;
			}
			while (cur1 != cur2) {
				cur1 = cur1.next;
				cur2 = cur2.next;
			}
			return cur1;
		} else {
			cur1 = loop1.next;
			while (cur1 != loop1) {
				if (cur1 == loop2) {
					return loop1;
				}
				cur1 = cur1.next;
			}
			return null;
		}
	}

	public Node getIntersectNode(Node head1, Node head2) {
		if (head1 == null || head2 == null) {
			return null;
		}
		Node loop1 = getLoopNode(head1);
		Node loop2 = getLoopNode(head2);
		if (loop1 == null && loop2 == null) {
			return noLoop(head1, head2);
		}
		if (loop1 != null && loop2 != null) {
			return bothLoop(head1, head2);
		}
		return null;
	}
}
```  
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%BA%8C%E7%AB%A0)

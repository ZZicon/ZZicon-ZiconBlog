---
layout:     post
title:      "数据结构最优解之链表Lab2.18"
subtitle:   "单链表的重新组合"
date:       2017-05-12 14:29:46
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “非连续非顺序、动态地进行存储分配的存储结构“

---

# 正文

**题目：按照左右半区的方式重新组合单链表**

要求是：给定一个单链表的头节点head，链表长度为N，将链表进行分区，左半区为L1->L2->...，右半区为R1->R2->...，重新组合为L1->R1->L2->R2。

**思路分析** 

 - 链表为空或长度小于等于2，不用调整。
 - 遍历链表找到左半区最后一个节点，记为mid。
 - 将左右半区以mid为界，分离成两个链表。
 - 合并链表。

**代码实现**

***链表节点***

```
class Node {
	public int value;
	public Node next;
	public Node random;

	public Node(int data) {
		this.value = data;
	}
}
```
  
***过程实现***

```
public class Lab2_18 {

	// 合并
	public Node merge(Node left, Node right) {
		Node next = null;
		while (left.next != null) {
			next = right.next;
			right.next = left.next;
			left.next = right;
			left = right.next;
			right = next;
		}
		left.next = right;
		return left;
	}

	public void relocate(Node head) {
		if (head == null || head.next == null || head.next.next == null) {
			return;
		}
		Node mid = head;
		Node right = head.next;
		while (right.next != null && right.next.next != null) {
			mid = mid.next;
			right = right.next.next;
		}
		right = mid.next;
		mid.next = null;
		head = merge(head, right);
	}
}
```  

至此，关于链表问题的总结归纳就已经全部完成了，下一章节Zicon将给大家介绍的是二叉树问题。

# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%BA%8C%E7%AB%A0)

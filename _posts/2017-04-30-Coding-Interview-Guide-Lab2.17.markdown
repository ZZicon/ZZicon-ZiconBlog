---
layout:     post
title:      "数据结构最优解之链表Lab2.17"
subtitle:   "环形链表插入新节点"
date:       2017-04-30 9:29:46
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “非连续非顺序、动态地进行存储分配的存储结构“

---

# 正文

**题目：向有序的环形单链表中插入新的节点**

要求是：一个环形单链表从头节点开始有序。给定一个环形单链表头节点head和一个整数num，插入值为num的新节点。

**思路分析** 

 - 先生成节点值为num的新节点，记为node
 - 当链表为空，让node自己组成环形链表，返回node
 - 链表不为空，记pre=head，cur=head.next。pre和cur同步下移，遇到pre节点值小于或等于num，并且cur节点大于或等于num，在pre与cur之间插入node。
 - 如果pre和cur没有找到满足的，说明node应该插入到头节点的前面。
 - 需要注意：node节点比链表中每个节点的值都大，返回原来的头节点；否则将node作为新的头节点返回。

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
public class Lab2_17 {
	public Node insertCirclr(Node head, int num) {
		Node node = new Node(num);
		if (head == null) {
			node.next = node;
			return node;
		}
		Node pre = head;
		Node cur = head.next;
		while (cur != head) {
			if (pre.value <= num && cur.value >= num) {
				break;
			}
			pre.next = node;
			node.next = cur;
			return head.value < num ? head : node;
		}
		return head;
	}
}
```  

# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%BA%8C%E7%AB%A0)

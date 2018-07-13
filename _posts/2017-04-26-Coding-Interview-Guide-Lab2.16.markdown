---
layout:     post
title:      "数据结构最优解之链表Lab2.16"
subtitle:   "链表的怪异删除"
date:       2017-04-26 20:29:46
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “非连续非顺序、动态地进行存储分配的存储结构“

---

# 正文

**题目：一种怪异的节点删除方式**

要求是：链表节点值是int类型，给定一个链表中的节点node，但不给定整个链表的头节点。应该如何在链表中删除node。

**思路分析** 

本题的思路很简单，将这个指针指向的next节点值copy到本节点，将next指向next->next,并随后删除原next指向的节点。

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
public class Lab2_16 {
	public void removeNode(Node node) {
		if (node == null) {
			return;
		}
		Node next = node.next;
		if (next == null) {
			System.out.println("无法删除最后一个节点。");
		}
		node.value = next.value;
		node.next = next.next;
	}
}
```  

在实现的代码中可以看到“无法删除最后一个节点”。这是为什么呢？

因为，这种删除方式在本质上不是对node节点的删除，而是把node节点的值改变，然后删除node下一个节点。
但是当node节点指向null的时候，我们不能通过将节点node在内存上的区域变成null实现删除。
因为null在系统中是一个特定的区域，要想让节点node的前一个节点指向null，只能先找到前一个节点
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%BA%8C%E7%AB%A0)

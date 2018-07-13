---
layout:     post
title:      "数据结构最优解之链表Lab2.15"
subtitle:   "链表与排序"
date:       2017-04-25 16:29:46
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “非连续非顺序、动态地进行存储分配的存储结构“

---

# 正文

**题目：对单链表进行选择排序**

要求是：给定一个无序单链表的头结点head，实现单链表的选择排序。要求空间复杂度为O(1)。

**思路分析** 

本题主要考核的是对链表的调整技巧。结合选择排序的思路。
 
具体分析如下： 
 
 - 开始默认链表未排序进行遍历，对于找到的最小值节点记为新的头结点。
 - 每次遍历未排序部分，然后把这个节点从未排序链表中删除。
 - 将删除的节点连接到排好序的部分链表的尾部。

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
public class Lab2_15 {

	// 查找最小节点前一个节点
	public Node getSmallNode(Node head) {
		Node smallNode = null, small = head;
		Node pre = head, cur = head.next;
		while (cur != null) {
			if (small.value > cur.value) {
				smallNode = pre;
				small = cur;
			}
			pre = cur;
			cur = cur.next;
		}
		return smallNode;
	}

	// 排序
	public Node selectionSort(Node head) {
		Node cur = head;
		Node sort = null, small = null, smallPre = null;
		while (cur != null) {
			smallPre = getSmallNode(cur);
			if (smallPre == null) {
				small = cur;
				cur = cur.next;

			} else {
				small = smallPre.next;
				smallPre.next = small.next;
			}
			if (sort == null) {
				head = small;
			} else {
				sort.next = small;
			}
			sort = small;
		}
		return head;
	}
}
```  

实际上，该题的复杂度取决于二叉树遍历，如果二叉树遍历的实现在时间与空间复杂度上足够好，那么本题就可以做到在复杂度上足够好。
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%BA%8C%E7%AB%A0)

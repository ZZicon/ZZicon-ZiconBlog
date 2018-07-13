---
layout:     post
title:      "数据结构最优解之链表Lab2.13"
subtitle:   "删除链表中值重复的节点"
date:       2017-04-24 19:29:46
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “非连续非顺序、动态地进行存储分配的存储结构“

---

# 正文

**题目：删除无序单链表中值重复出现的节点**

要求是：给定单链表头节点head，删除其中值重复出现的节点。

例如：链表1->2->3->3->4->4->2->1->null个例子。调整后为，1->2->3->4->null。

**思路分析** 

最简单的实现方式是遍历删除，遍历删除时间复杂度为O(N^2)，空间复杂度为O(1)。在这里就不分析该方法。

而我们可以考虑使用数据结构实现，比如利用哈希表实现。时间复杂度为O(N)，空间复杂度为O(N)。
 
方法二的具体分析如下： 
 
 - 建立哈希表。将头节点的值放入哈希表。
 - 遍历链表，假设当前节点为cur，检验cur的值是否存在于哈希表中。
 - 如果存在则说明cur节点的值是重复的，删除cur节点。如果不存在则将cur的值加入哈希表。

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
import java.util.HashSet;

public class Lab2_13 {
	// 方法一：遍历删除
	public void removeRep1(Node head) {
		if (head == null) {
			return;
		}
		Node cur = head;
		Node pre = null, next = null;
		while (cur != null) {
			pre = cur;
			next = cur.next;
			while (next != null) {
				if (cur.value == next.value) {
					pre.next = next.next;
				} else {
					pre = next;
				}
				next = next.next;
			}
			cur = cur.next;
		}
	}

	// 方法二：哈希表方法
	public void removeRep2(Node head) {
		if (head == null) {
			return;
		}
		HashSet <Integer> hash = new HashSet <Integer>();
		Node pre = head;
		Node cur = head.next;
		hash.add(head.value);
		while (cur != null) {
			if (hash.contains(cur.value)) {
				pre.next = cur.next;
			} else {
				hash.add(cur.value);
				pre = cur;
			}
			cur = cur.next;
		}
	}
}
```  
 
**删除节点扩展**——遍历删除指定值的节点

时间复杂度为O(N)，空间复杂度为O(1)

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
public Node removeValue(Node head, int num) {
	if (head == null || num < 1) {
		return head;
	}

	// 获取第一个非num节点
	while (head != null) {
		if (head.value == num) {
			head = head.next;
		} else {
			break;
		}
	}

	Node pre = head;
	Node cur = head;
	while (cur != null) {
		if (cur.value == num) {
			pre.next = cur.next;
		} else {
			pre = cur;
		}
		cur = cur.next;
	}
	return head;
}
```
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%BA%8C%E7%AB%A0)

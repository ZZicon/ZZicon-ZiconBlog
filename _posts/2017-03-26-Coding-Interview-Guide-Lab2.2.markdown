---
layout:     post
title:      "数据结构最优解之链表Lab2.2"
subtitle:   "链表删除倒数第K个节点"
date:       2017-03-26 17:28:16
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “非连续非顺序、动态地进行存储分配的存储结构“

---

# 正文

**题目：在链表中删除倒数第K个节点**

要求是：删除链表节点，链表长度为N，时间复杂度为O(N)，额外空间复杂度为O(1)。

**思路分析**
 
 - 链表从头开始走到尾，每移动一步，K-1。
 - K>0说明不存在该节点；K=0说明要求的是头结点。
 - K<0时，要确认删除节点的前一个节点，方便进行删除操作。
 - 重新从头结点出发，每移动一步，K+1，当K=0时该节点就是要删除节点的前一个节点。
 
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
public class Lab2_2 {
	//单链表
	public Node removeLastNode(Node head, int n) {
		if (head == null || n < 1) {
			return head;
		}
		Node fast = head;
		Node slow = head;
		if (fast.next == null) {
			head = head.next;
		}
		while (n > 0) {
			fast = fast.next;
			n--;
		}
		while (fast.next != null) {
			fast = fast.next;
			slow = slow.next;
		}
		slow.next = slow.next.next;
		return head;
	}
	
	//双链表
	public DoubleNode removeLastNode(DoubleNode head, int n){
		if (head == null || n < 1) {
			return head;
		}
		DoubleNode fast = head;
		DoubleNode slow = head;
		if (fast.next == null) {
			head = head.next;
			head.last = null;
		}
		while (n > 0) {
			fast = fast.next;
			n--;
		}
		while (fast.next != null) {
			fast = fast.next;
			slow = slow.next;
		}
		slow.next = slow.next.next;
		if(slow.next!=null){
			slow.next.last=slow;
		}
		return head;
	}
}
```  
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%BA%8C%E7%AB%A0)

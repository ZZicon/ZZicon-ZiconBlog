---
layout:     post
title:      "数据结构最优解之链表Lab2.10"
subtitle:   "链表相加"
date:       2017-04-23 13:29:46
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “非连续非顺序、动态地进行存储分配的存储结构“

---

# 正文

**题目：两个单链表生成相加链表**

要求是：给定两个单链表，其中每一个节点的值都在0~9之间，整个链表可以代表整数，生成代表两个整数相加的结果的链表。

**适用于大数据相加哟**

**思路分析** 
 
 - 使用栈结构，通过栈的一次压入弹出进行一次运算。
 - 记录进位进行下一次运算。
 - 运算完成后进行新链的生成。

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
public class Lab2_10 {
	public Node addListNode(Node head1, Node head2) {
		if (head1 == null) {
			return head2;
		}
		if (head2 == null) {
			return head1;
		}
		int flag = 0;
		Stack <Node> stack1 = new Stack <Node>();
		Stack <Node> stack2 = new Stack <Node>();
		while (head1 != null) {
			stack1.push(head1);
			head1 = head1.next;
		}
		while (head2 != null) {
			stack2.push(head2);
			head2 = head2.next;
		}

		Node add = null;
		Node pre = null;
		while (!stack1.isEmpty() || !stack2.isEmpty()) {
			int s1 = stack1.isEmpty() ? 0 : stack1.pop().value;
			int s2 = stack2.isEmpty() ? 0 : stack2.pop().value;
			int sum = s1 + s2 + flag;
			int value = sum % 10;
			flag = sum / 10;
			pre = add;
			add = new Node(value);
			add.next = pre;
		}
		if (flag == 1) {
			pre = add;
			add = new Node(1);
			add.next = pre;
		}
		return add;
	}
}
```  
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%BA%8C%E7%AB%A0)

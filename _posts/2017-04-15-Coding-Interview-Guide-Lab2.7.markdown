---
layout:     post
title:      "数据结构最优解之链表Lab2.7"
subtitle:   "判断链表回文"
date:       2017-04-15 15:39:46
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “非连续非顺序、动态地进行存储分配的存储结构“


---

# 正文

**题目：判断一个链表是否回文结构**

要求是：一个环形单链表头结点head，判断其是不是回文结构。这个类型的题目解法很多，只要运用到栈结构一般实现都没有问题。那么Zicon在这里就放出几种解法，其中只介绍最难最麻烦的一种。
 
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
public class Lab2_7 {
	// 一般解法
	public boolean isPalindromNormal(Node head) {
		if (head == null || head.next == null) {
			return true;
		}
		Stack <Node> stack = new Stack <Node>();
		Node cur = head;
		while (cur != null) {
			stack.push(cur);
			cur = cur.next;
		}
		while (head != null) {
			if (head.value != stack.pop().value) {
				return false;
			}
			head = head.next;
		}
		return true;
	}

	// 进阶解法
	public boolean isPalindromUp(Node head) {
		if (head == null || head.next == null) {
			return true;
		}
		Stack <Node> stack = new Stack <Node>();
		Node cur = head;
		Node right = head.next;
		while (cur.next != null && cur.next.next != null) {
			right = right.next;
			cur = cur.next.next;
		}
		while (right != null) {
			stack.push(right);
			right = right.next;
		}
		while (!stack.isEmpty()) {
			if (head.value != stack.pop().value) {
				return false;
			}
			head = head.next;
		}
		int n = 0;
		return true;
	}

	// 空间复杂度最小的解
	public boolean isPalindromComplex(Node head) {
		if (head == null || head.next == null) {
			return true;
		}

		// 保存结果
		boolean result = true;

		// 获取中间节点mid
		Node mid = head, node1 = head;
		while (node1.next != null && node1.next.next != null) {
			mid = mid.next;
			node1 = node1.next.next;
		}

		// 右半区逆序
		node1 = mid.next; // 右半区第一个节点
		mid.next = null; // 中间节点指向null
		Node next = null;
		while (node1 != null) {
			next = node1.next;
			node1.next = mid;
			mid = node1;
			node1 = next;
		}

		// 左右进行比较
		Node lnode = head;
		Node rnode = mid;
		while (lnode != null && rnode != null) {
			if (lnode.value != rnode.value) {
				result = false;
			}
			lnode = lnode.next;
			rnode = rnode.next;
		}

		// 恢复
		node1 = mid.next;
		mid.next = null;// 最后一个节点指向null
		while (node1 != null) {
			next = node1.next;
			node1.next = mid;
			mid = node1;
			node1 = next;
		}

		return result;
	}
	
	//常见解法
	public boolean isPalindromEasy(Node head) {
		if (head == null || head.next == null) {
			return true;
		}
		Node cur = head;
		Stack <Node> stack = new Stack <Node>();
		int n = 0;
		while (cur != null) {
			n++;
			cur = cur.next;
		}
		int mid = n / 2;
		while (mid != 0) {
			stack.push(head);
			head = head.next;
			mid--;
		}
		if (n % 2 == 1) {
			head = head.next;
		}
		while (head != null) {
			if (head.value != stack.pop().value) {
				return false;
			}
			head = head.next;
		}
		return true;
	}

}
```  

上述解法中：

 - 一般解法是将所有节点入栈，然后借助栈先进后出的特性与原链表进行比较。
 - 常见解法是把链表分两个部分进行左右对比。
 - 进阶解法是对一般解法和常见解法的优化，不仅只需要一半的节点入栈，而且实现比常见解法简便。
 - 空间复杂度最小的解，其额外空间复杂度为O(1)。

空间复杂度最小的解之所以使用的额外空间小，是因为它不需要使用栈和其他的数据结构。

**思路分析** 
 
 - 将链表右半区反转，指向中间节点。
 - 左半区节点第一个节点记为lstart，右半区第一个节点记为rstart。
 - 两个节点同时向中间节点移动并对比节点的值。如果都一样则说明链表是回文结构。
 - 将链表恢复原来的结构。

 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%BA%8C%E7%AB%A0)

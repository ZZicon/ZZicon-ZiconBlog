---
layout:     post
title:      "数据结构最优解之链表Lab2.12"
subtitle:   "链表逆序特殊情况"
date:       2017-04-24 17:29:46
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “非连续非顺序、动态地进行存储分配的存储结构“

---

# 正文

**题目：单向链表每k个数逆序拼接**

要求是：给定单链表头节点head，使得每K个节点之间逆序，如果最后不够K个节点一组，则不调整最后几个节点。

例如：链表1->2->3->4->5->6->7->8->null，K=3这个例子。调整后为，3->2->1->6->5->4->7->8->null。

**思路分析** 

Zicon在这里利用栈实现。需要注意的是K值少于2时，不用对链表进行调整。使用栈解法的时间复杂度为O(N)，空间复杂度为O(K)。
 
具体分析如下： 
 
 - 遍历链表，当栈大小不等于K，将节点压入栈。
 - 栈大小等于K的时候进行逆序，需要注意的是逆序后要记录新的头节点。同时第一组的最后一个节点要连接下一个节点。
 - 每一次逆序完成之后，当前组的第一个节点要被上一组的最后一个节点连接，同时这一组最后一个节点连接下一个节点。

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
import java.util.Stack;

public class Lab2_12 {

	public Node reversrKNode(Node head, int K) {
		if (head == null || K < 2) {
			return head;
		}
		Stack <Node> stack = new Stack <Node>();
		Node newHead = head;
		Node cur = head;
		Node pre = null, next = null;
		while (cur != null) {
			next = cur.next;
			stack.push(cur);
			if (stack.size() == K) {
				// 逆序操作
				pre = reset(stack, pre, next);
				// 重点！更新头结点
				newHead = newHead == head ? cur : newHead;
			}
			cur = cur.next;
		}
		return newHead;
	}

	// 逆序
	public Node reset(Stack <Node> stack, Node pre, Node next) {
		Node cur = stack.pop();
		if (pre != null) {
			pre.next = cur;
		}
		Node nextNode = null;
		while (!stack.isEmpty()) {
			nextNode = stack.pop();
			cur.next = nextNode;
			cur = nextNode;
		}
		cur.next = next;
		return cur;
	}
}
```  
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%BA%8C%E7%AB%A0)

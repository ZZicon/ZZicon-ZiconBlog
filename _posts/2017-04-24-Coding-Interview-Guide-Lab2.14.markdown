---
layout:     post
title:      "数据结构最优解之链表Lab2.14"
subtitle:   "将二叉树转换为双向链表"
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

**题目：将搜索二叉树转换成双向链表**

要求是：对每一个节点来说，原来的right指针等价于转换后的next指针，原来的left指针等价于转换后的last指针，最后返回转换后的双向链表头节点。

**思路分析** 

Zicon在这里使用队列实现，时间复杂度为O(N)，空间复杂度为O(N)。
 
具体分析如下： 
 
 - 生成队列，按照二叉树中序遍历的顺序将节点放入queue。
 - 从queue中依次弹出节点，并按照弹出顺序重连节点。

**代码实现**

***链表节点***

```
class Node {
	public int value;
	public Node left;
	public Node right;

	public Node(int data) {
		this.value = data;
	}
}
```
  
***过程实现***

```
public class Lab2_14 {

	// 中序遍历
	public void inOrderToQueue(Node head, Queue <Node> queue) {
		if (head == null) {
			return;
		}
		inOrderToQueue(head.left, queue);
		queue.offer(head);
		inOrderToQueue(head.right, queue);
	}

	public Node exchange(Node1 head) {
		Queue <Node> queue = new LinkedList <Node>();
		inOrderToQueue(head, queue);
		if (queue.isEmpty()) {
			return head;
		}
		head = queue.poll();
		Node pre = head;
		pre.left = null;
		Node cur = null;
		while (!queue.isEmpty()) {
			cur = queue.poll();
			pre.right = cur;
			cur.left = pre;
			pre = cur;
		}
		pre.right = null;
		return head;
	}
}
```  

实际上，该题的复杂度取决于二叉树遍历，如果二叉树遍历的实现在时间与空间复杂度上足够好，那么本题就可以做到在复杂度上足够好。
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%BA%8C%E7%AB%A0)

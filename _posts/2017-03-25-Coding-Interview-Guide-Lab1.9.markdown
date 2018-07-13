---
layout:     post
title:      "数据结构最优解之栈与队列Lab9"
subtitle:   "数值差符合一定大小的子数组"
date:       2017-03-25 15:38:16
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “程序设计中被广泛使用的两种线性结构“

---

# 正文

**题目：求最大值与最小值的差小于或等于num的子数组数量**

要求是：对于数组arr和整数num，共返回有多少个子数组满足于max - min <= num。设数组长度为N，时间复杂度为O(N)。

**思路分析**

***一般思路***

 - 对数组所有子数组遍历寻找最大最小值，判断是否符合要求。
 - 时间复杂度为O(N^3)。

***进阶思路***
 
进阶思路的关键在于利用双端队列实现最大值与最小值的更新。时间复杂度为O(N)，额外空间复杂度为O(N)。双端队列的结构与之前[生成窗口最大值数组](https://zzicon.github.io/ZiconBlog/2017/03/24/Coding-Interview-Guide-Lab1.6/)相似。

 - 生成两个双端队列qmax与qmin，以两个整型变量i、j表示子数组范围。
 - 令j++，不断更新qmax与qmin结构，直到出现arr[i..j]不满足题目要求，停止j++，并记录下以arr[i]为开头的满足条件子数组个数j-i。
 - 完成步骤2时，i++，重复步骤2。
 - 根据步骤3、4累加符合条件的子数组个数。

 
**代码实现**

***一般思路***

```
public class Lab1_10 {
	public int getNum(int[] arr, int num) {
		if (arr.length == 0 || arr == null) {
			return 0;
		}
		LinkedList <Integer> qmin = new LinkedList <Integer>();
		LinkedList <Integer> qmax = new LinkedList <Integer>();
		int i = 0, j = 0, res = 0;
		while (i < arr.length) {
			while (j < arr.length) {
				while (!qmin.isEmpty()
						&& arr[qmin.peekLast()] >= arr[j]) {
					qmin.pollLast();
				}
				qmin.addLast(j);
				while (!qmax.isEmpty()
						&& arr[qmax.peekLast()] <= arr[j]) {
					qmax.pollLast();
				}
				qmax.addLast(j);
				if (arr[qmax.getFirst()] - arr[qmin.getFirst()] > num) {
					break;
				}
				j++;
			}
			if (qmin.peekFirst() == i) {
				qmin.pollFirst();
			}
			if (qmax.peekFirst() == i) {
				qmax.pollFirst();
			}
			res += j - i;
			i++;
		}
		return res;
	}
}
```
  
 
# 后记
到这里为止，关于栈与队列的章节就完成了，接下来Zicon将会进行第二章的学习与总结。

Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%B8%80%E7%AB%A0)

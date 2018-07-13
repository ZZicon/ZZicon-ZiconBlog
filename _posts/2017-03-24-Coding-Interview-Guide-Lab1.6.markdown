---
layout:     post
title:      "数据结构最优解之栈与队列Lab6"
subtitle:   "生成窗口最大值数组"
date:       2017-03-24 10:24:26
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “程序设计中被广泛使用的两种线性结构“

---

# 正文

**题目：生成窗口的最大值数组**

要求是：一个整数数组arr和一个大小为w的窗口从数组最左边滑到最右边，返回过程中，每一次窗口移动时生成的最大值。

提示：假设数组长度为n，窗口大小为w，则一共产生n-w+1个最大值。

**思路分析**

***一般思路***

 - 常规思路是随着窗口的移动，遍历窗口内数组找出最大值，但是这种实现方法的时间复杂度为O(N*W)。
 - 进阶思路是要满足时间复杂度为O(N)，即与窗口大小无关。
 
进阶思路的关键在于利用双端队列实现最大值的更新。qmax是一个双端队列,，存放arr数组的下标。遍历数组到arr[i]时，qmax的压入规则为： 

 - 如果qmax为空，直接把下标放入qmax队列，结束。
 - 如果qmax不为空，取出当前qmax队尾存放的下标，假设为j。
 - 如果num[j]>num[i]，直接把下标放入qmax的队尾，结束。 
 - 如果num[j]<=num[i]，把j从qmax中弹出，继续qmax的存放规则。
 
遍历数组到arr[i]时，qmax的弹出规则为：

 - 如果qmax队头的下标等于i-w，说明当前qmax的队头下标已过期，弹出当前队头的下标即可。
 
**代码实现**

***一般思路***

```
	public int getMax(int n1, int n2, int n3) {
		int max = 0;
		if (n1 > n2) {
			max = n1;
		} else {
			max = n2;
		}
		max = (max > n3) ? max : n3;
		return max;
	}

	public int[] function1(int arr[], int w) {
		int answer[] = new int[arr.length + -w + 1];
		for (int i = 0; i < arr.length - w + 1; i++) {
			answer[i] = getMax(arr[i], arr[i + 1], arr[i + 2]);
		}
		return answer;
	}
```

***双端队列***

```
public class Lab1_7 {
	public int[] function2(int[] arr, int w) {
		if (arr == null || w < 1 || arr.length < w) {
			return null;
		}
		LinkedList <Integer> maxqueue = new LinkedList <Integer>();
		int[] res = new int[arr.length - w + 1];
		int index = 0;
		for (int i = 0; i < arr.length; i++) {
			//存大值下标，将大值存放入qmax
			while (!maxqueue.isEmpty() && arr[maxqueue.peekLast()] <= arr[i]) {
				maxqueue.pollLast();
			}
			maxqueue.addLast(i);
			if (maxqueue.peekFirst() == i - w) {
				//超出窗口范围，去掉队列头
				maxqueue.pollFirst();
			}
			if (i >= w - 1) {
				//从窗口大小w处开始记录结果值（下标都是从0开始，故从w-1处开始存储）
				res[index++] = arr[maxqueue.peekFirst()];
			}
		}
		return res;
	}
}

```
  
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%B8%80%E7%AB%A0)

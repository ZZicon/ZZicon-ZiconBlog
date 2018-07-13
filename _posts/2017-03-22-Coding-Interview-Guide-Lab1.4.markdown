---
layout:     post
title:      "数据结构最优解之栈与队列Lab4"
subtitle:   "猫狗队列"
date:       2017-03-22 16:31:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “程序设计中被广泛使用的两种线性结构“


# 前言

第四题来了。

---

# 正文

**题目：猫狗队列**

相关类：

```
class Pet {
	private String type;

	public Pet(String type) {
		this.type = type;
	}

	public String getPetType() {
		return this.type;
	}
}

class Dog extends Pet {
	public Dog() {
		super("dog");
	}
}

class Cat extends Pet {
	public Cat() {
		super("cat");
	}
}
```

要求是：

 - add方法可以将cat或dog类的实例放入队列中。
 - pollAll方法将队列中所有的实例按照进队列的先后顺序依次弹出。
 - pollDog方法将队列中dog类按进队列顺序弹出。
 - pollCat方法与pollDog方法类似。
 - isEmpty方法判断队列是否还有dog或cat类的实例。
 - isDogEmpty方法针对dog实例，isCatEmpty针对cat实例。
 
题目初看之下体现不出难度，但是在实现方法的时候不能改变题目给出的类。而要实现各类按顺序弹出就需要一个时间戳标记不同的实例。

因此，需要创建一个新的类，满足类中有存放不同类型实例的队列，并且有一个时间戳进行标记。

```
class PetEnterQueue {
	private Pet pet;
	private long count;

	public PetEnterQueue(Pet pet, long count) {
		this.pet = pet;
		this.count = count;
	}

	public Pet getPet() {
		return this.pet;

	}

	public long getCount() {
		return this.count;

	}

	public String getEnterType() {
		return this.pet.getPetType();
	}
}
```

只要实现了这个类，后续的对弹出以及判断是否为空的问题就很清晰了。
 - 压入实例时加上时间戳后按类型的不同分别进入不同的队列。
 - 弹出单一类型时只针对对应的队列操作。
 - 按总的顺序弹出时，只要比较两个队列的头元素的时间戳即可。
 - 至于判断为空就更简单了，只用对队列进行isEmpty判断。

**代码实现**

```
public class Lab1_4 {
	private Queue <PetEnterQueue> dogQueue;
	private Queue <PetEnterQueue> catQueue;
	private long count;

	public Lab1_4() {
		this.dogQueue = dogQueue;
		this.catQueue = catQueue;
		this.count = 0;
	}

	public void add(Pet pet) {
		if (pet.getPetType().equals("dog")) {
			this.dogQueue.add(new PetEnterQueue(pet, this.count++));
		} else if (pet.getPetType().equals("cat")) {
			this.catQueue.add(new PetEnterQueue(pet, this.count++));
		} else {
			throw new RuntimeException("error,not dog or cat");
		}
	}

	public Pet pollAll() {
		if (!this.dogQueue.isEmpty() && !this.catQueue.isEmpty()) {
			if (this.dogQueue.peek().getCount() < this.catQueue.peek()
					.getCount()) {
				return this.dogQueue.poll().getPet();
			} else {
				return this.catQueue.poll().getPet();
			}
		} else if (!this.dogQueue.isEmpty()) {
			return this.dogQueue.poll().getPet();
		} else if (!this.catQueue.isEmpty()) {
			return this.catQueue.poll().getPet();
		} else {
			throw new RuntimeException("error,queue is empty");
		}
	}

	public Dog pollDog() {
		if (!this.dogQueue.isEmpty()) {
			return (Dog) this.dogQueue.poll().getPet();
		} else {
			throw new RuntimeException("Dog queue is empty");
		}
	}

	public Cat pollCat() {
		if (!this.catQueue.isEmpty()) {
			return (Cat) this.catQueue.poll().getPet();
		} else {
			throw new RuntimeException("Cat queue is empty");
		}
	}

	public boolean isEmpty() {
		return this.dogQueue.isEmpty() && this.catQueue.isEmpty();
	}

	public boolean isDogQueueEmpty() {
		return this.dogQueue.isEmpty();
	}

	public boolean isCatQueueEmpty() {
		return this.catQueue.isEmpty();
	}
}
```
  
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%B8%80%E7%AB%A0)

---
layout:     post
title:      "Android随笔之初心者的RxJava总结"
subtitle:   "关于Android开发的相关知识点"
date:       2018-09-29 10:47:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - Android随笔
---

> “初步接触RxJava的总结”


# 前言

笔者在项目的推动下要使用RxJava，因此先去了解一下RxJava的一些基础知识。

当然，这是笔者自己的总结，参考了几篇通俗易懂的文章，会在文末贴出链接。

---

# 正文

RxJava有以下几个关键词：链式结构、异步操作、简洁、操作符

其实现步骤有：创建上游、创建下游、上下游连通，以及在连通过程中使用操作符进行相关处理

具体为：

 **添加依赖**
 
```
compile 'io.reactivex.rxjava2:rxjava:2.0.1'
compile 'io.reactivex.rxjava2:rxandroid:2.0.1'
```

 **创建上游**
 
 上游指的是在RxJava中负责发送事件的Observable，他通过调用onNext()、onCompleted()、onError()方法，决定什么时候触发事件以及触发怎样的事件。
 
 **创建下游**
 
 下游指的是在RxJava中负责接收事件的Observer，他通过实现onNext()、onCompleted()、onError()方法，决定对触发的事件做何种处理。
 
 **订阅（连通）**
 
 Observable调用subscribe方法连通Observer

 **实例**
 
 ```
//创建一个上游 Observable：
Observable<Integer> observable = Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> emitter) throws Exception {
        for (int i = 0; i < 5; i++) {
            emitter.onNext(i + 1);
        }
        emitter.onComplete();
    }
});
//创建一个下游 Observer
Observer<Integer> observer = new Observer<Integer>() {
    @Override
    public void onSubscribe(Disposable d) {
        Log.d(TAG, "subscribe");
    }

    @Override
    public void onNext(Integer value) {
        Log.d(TAG, "" + value);
    }

    @Override
    public void onError(Throwable e) {
        Log.d(TAG, "error");
    }

    @Override
    public void onComplete() {
        Log.d(TAG, "complete");
    }
};
//建立连接
observable.subscribe(observer);	
 ```
 
 **链式化**
 
 ```
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> emitter) throws Exception {
        for (int i = 0; i < 5; i++) {
            emitter.onNext(i + 1);
        }
        emitter.onComplete();
    }
}).subscribe(new Observer<Integer>() {
    @Override
    public void onSubscribe(Disposable d) {
        Log.d(TAG, "subscribe");
    }

    @Override
    public void onNext(Integer value) {
        Log.d(TAG, "" + value);
    }

    @Override
    public void onError(Throwable e) {
        Log.d(TAG, "error");
    }

    @Override
    public void onComplete() {
        Log.d(TAG, "complete");
    }
}); 
 ```
 
 **线程控制**
 
 在链式结构的任意位置调用以下两个方法即可控制上下游的线程。

 ```
 // 指定订阅发生的线程，也可以理解为上游发送事件的线程
 .subscribeOn(Schedulers.newThread())
 // 指定下游接收事件的线程
 .observeOn(AndroidSchedulers.mainThread())
 ```
 
 **注意事项**
 
 - 上游可以发送无数onNext，下游也可以接收无数个onNext
 - 上游发送了一个onComplete、onError后可以继续发送其他事件，但是下游接收到onComplete、onError后不再接收其他事件
 - 上游可以不发送onComplete、onError事件
 - onComplete和onError事件唯一且互斥
 - subscribeOn指定上游发送事件的线程，可以多次调用但只有第一次设置的才有效其余会被忽略
 - observeOn指定下游接收事件的线程，每次调用后下游的线程都会进行切换
  
---

# 后记
以上就是笔者自己对RxJava的初步认识，同时给出链接：

在这位作者的动态里有教程一到十，可以自行阅读：[给初学者的RxJava2.0教程（一）](https://www.jianshu.com/p/464fa025229e)

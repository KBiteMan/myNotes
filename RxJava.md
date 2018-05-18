---
title: RxJava
tags:响应式变成
grammar_cjkRuby: true
---


> RxJavas是基于观察者模式的，所以你将会看到很多观察者模式的影子

## Observer（观察者）

> 它决定事件触发的时候将有怎样的行为

#### 创建观察者
我们直接可以使用Observer去创建观察者

```java
Observer<String> observer = new Observer<String>() {

            @Override
            public void onCompleted() {
                System.out.println("完成");
            }

            @Override
            public void onError(Throwable throwable) {
                System.out.println("出错: " + throwable.getMessage().toString());
            }

            @Override
            public void onNext(String s) {
                System.out.println("成功： " + s);
            }
        };
```

你也可以Observer的实现的抽象类Subscriber去新建观察者，Subscriber对Observer借口进行了功能扩展

```java
Subscriber<String> subscriber = new Subscriber<String>() {
//.........
}；
```
- onStart()：这是 Subscriber 增加的方法。它会在 subscribe 刚开始，而事件还未发送之前被调用，可以用于做一些准备工作。但是它总是在subscribe所发生的线程中被调用，且不能指定线程
- unsubscribe()：用于取消订阅。在 subscribe() 之后， Observable 会持有 Subscriber的引用，这个引用如果不能及时被释放，将有内存泄露的风险

## Observable（被观察者）
> 它决定什么时候触发事件以及触发怎样的事件

### 创建Observable

```java
Observable<String> observable = Observable.create(new Observable.OnSubscribe<String>() {
            @Override
            public void call(Subscriber<? super String> subscriber) {
                subscriber.onNext("你好");
                subscriber.onNext("你也好");
                subscriber.onNext("最近在忙什么？");
                subscriber.onNext("我最近没忙什么，就是很难过");
                subscriber.onCompleted();
            }
        });
```
create() 方法是 RxJava 最基本的创造事件序列的方法。基于此还提供其他创建方式。

- just(T...):将传入的参数依次发送。

```java
Observable observable = Observable.just("Hello", "Hi", "Aloha");
// 将会依次调用：
// onNext("Hello");
// onNext("Hi");
// onNext("Aloha");
// onCompleted();
```

- from(T[]) / from(Iterable<? extends T>) : 将传入的数组或 Iterable 拆分成具体对象后，依次发送出来。

```java
String[] words = {"Hello", "Hi", "Aloha"};
Observable observable = Observable.from(words);
// 将会依次调用：
// onNext("Hello");
// onNext("Hi");
// onNext("Aloha");
// onCompleted();
```

## 订阅
既然有了观察者和订阅者，那就还得需要订阅这个动作。

```java
observable.subscribe(observer);
// 或者：
observable.subscribe(subscriber);
```
### ActionX类
通过ActionX，RxJava会自动根据定义创建出Subscriber
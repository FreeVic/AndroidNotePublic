# RxJava

[TOC]

## 命名

响应式编程就是与异步数据流交互的编程范式

Observable(可观察的,即被观察者) do sth

Observer(观察者)

Observable subscribe Observer

## 操作符

1. ### 创建

   1. `Create`/`Defer`/`From`/`Just`/`Start`/`Repeat`/`Range`

2. ### 变换

   1.  `Buffer`/`Window`/`Map`/`FlatMap`/`GroupBy`/`Scan`

3. ### 过滤

   1. `Debounce`/`Distinct`/`Filter`/`Sample`/`Skip`/`Take`

4. ### 结合

   1. `And`/`StartWith`/`Join`/`Merge`/`Switch`/`Zip`

5. ### 错误处理

   1. `Catch`/`Retry`

6. ### 辅助操作

   1. `Delay`/`Do`/`ObserveOn`/`SubscribeOn`/`Subscribe`

7. ### 条件和布尔

   1. `All`/`Amb`/`Contains`/`SkipUntil`/`TakeUntil`

8. ### 算数和聚合

   1. `Average`/`Concat`/`Count`/`Max`/`Min`/`Sum`/`Reduce`

9. ### 异步

   1. `Start`/`ToAsync`/`StartFuture`/`FromAction`/`FromCallable`/`RunAsync`

10. ### 连接

   1. `Connect`/`Publish`/`RefCount`/`Replay`

11. ### 转换

    1. `ToFuture`/`ToList`/`ToIterable`/`ToMap`/`toMultiMap`

12. ### 阻塞

    1. `ForEach`/`First`/`Last`/`MostRecent`/`Next`/`Single`/`Latest`

13. ### 字符串

    1. `ByLine`/`Decode`/`Encode`/`From`/`Join`/`Split`/`StringConcat`

## 线程切换

```
Observable
.map                    // 操作1
.flatMap                // 操作2
.subscribeOn(io)
.map                    //操作3
.flatMap                //操作4
.observeOn(main)
.map                    //操作5
.flatMap                //操作6
.subscribeOn(io)        //!!特别注意
.subscribe(handleData)
```

假设我们是在主线程上调用这段代码，那么

```
操作1，操作2是在 io 线程上，因为之后subscribeOn切换了线程
操作3，操作4也是在 io 线程上，因为在subscribeOn切换了线程之后，并没有发生改变。
操作5，操作6是在 main 线程上，因为在他们之前的observeOn切换了线程。
特别注意那一段，对于操作5和操作6是无效的
```

### 所以，总结起来就是

1. `subscribeOn`的调用切换之前的线程。
2. `observeOn`的调用切换之后的线程。
3. `observeOn`之后，不可再调用`subscribeOn` 切换线程
4. 下面提到的 “操作” 包括产生事件、用操作符操作事件以及最终的通过 subscriber 消费事件
5. 只有第一 subscribeOn() 起作用（所以多个 subscribeOn() 毛意义）
6. 这个 subscribeOn() 控制从流程开始的第一个操作，直到遇到第一个 observeOn()
7. observeOn() 可以使用多次，每个 observeOn() 将导致一次线程切换 ()，这次切换开始于这次 observeOn() 的下一个操作
8. 不论是 subscribeOn() 还是 observeOn()，每次线程切换如果不受到下一个 observeOn() 的干预，线程将不再改变，不会自动切换到其他线程

## 注意

1. 某些情况下不需要调用subscription.request(n); 例如Flowable.just()
2. ​

## 参考

1. [迷之RxJava线程切换](http://blog.csdn.net/jikeehuang/article/details/51454085)
2. [Awesome-RxJava资源合集](https://github.com/lzyzsd/Awesome-RxJava)
3. [从零开始的RxJava2.0](http://blog.csdn.net/qq_35064774/article/details/53057332)


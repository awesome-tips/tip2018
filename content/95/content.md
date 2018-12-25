## 判断是否在主队列运行

**作者**: [南峰子](https://weibo.com/touristdiary)

在 iOS 中，如果我们要判断代码是否运行在主线程，可以直接使用`NSThread.isMainThread()`方法。但如果要判断是否运行在主队列(main queue)呢？

需要注意的是，每个应用都只有一个主线程，但主线程中可能有多个队列，则不仅仅只有主队列，所以 `NSThread.isMainThread()` 方法并没有办法判断是否是在主队列运行。而GCD也没有提供相应的方法。那该如何处理呢？

来看看 React Native 的处理方式：

```objc
BOOL RCTIsMainQueue()
{
  static void *mainQueueKey = &mainQueueKey;
  static dispatch_once_t onceToken;
  dispatch_once(&onceToken, ^{
    dispatch_queue_set_specific(dispatch_get_main_queue(),
                                mainQueueKey, mainQueueKey, NULL);
  });
  return dispatch_get_specific(mainQueueKey) == mainQueueKey;
}
```

可以看到这里使用了 `dispatch_queue_set_specific` 和 `dispatch_get_specific` 。实际上是通过 `dispatch_queue_set_specific` 方法给主队列(main queue)关联了一个 `key-value` 对，再通过 `dispatch_get_specific` 从当前队列中取出 `mainQueueKey` 对应的 value。如果是主队列，取出来的值就是写入的值，如果是其它主队列，取出的值就是另一个值或者是 NULL。这样就 OK 了。

当然，同样可以用类似的方式来设置其它的队列(设置全局并发队列无效)。


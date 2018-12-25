## 定时器引发的思考

**作者**: [Lefe_x](https://weibo.com/u/5953150140)

当使用 `AudioServicesPlaySystemSoundWithCompletion` 播放完一段音频后，在回调中开启一个定时器，然后发现定时器不执行，代码是这样的：

```objc
AudioServicesPlaySystemSoundWithCompletion(sysSoundID, ^{
    AudioServicesDisposeSystemSoundID(sysSoundID);
    NSTimer *timer = [NSTimer scheduledTimerWithTimeInterval:1.0 target:self selector:@selector(lefexTimerAction:) userInfo:nil repeats:YES];
});

- (void)lefexTimerAction:(NSTimer*)timer
{
   // 这个方法并不会执行
}
```

如果我把代码改成下面这样，定时器可以正常执行，但是发现方法 `lefexTimerAction:` 并不在主线程中执行，而是在开启定时器对应的线程中执行，代码如下：

```
AudioServicesPlaySystemSoundWithCompletion(sysSoundID, ^{
   AudioServicesDisposeSystemSoundID(sysSoundID);
   NSTimer *timer = [NSTimer scheduledTimerWithTimeInterval:1.0 target:self selector:@selector(handlePageStayTimer:) userInfo:nil repeats:YES];
   [[NSRunLoop currentRunLoop] run];
 });
```

看到这里，相信你已经明白是什么原因了。

系统音频播放完成后回调并不在主线程，导致开启定时器时不在主线程。`Timer` 和 `runLoop` 是一起工作的，没有 `runLoop`，定时器不能正常执行。而系统中只有 `mainRunLoop` 会默认开启，这就是为什么在主线程创建定时器可以正常执行的原因。

`runLoop` 会强引用 `timer`，这就是我们经常所说的为什么 `timer` 会导致内存泄漏，即使在 `dealloc` 中释放 `timer`，也不能避免内存泄漏，因为 `dealloc` 就不会执行。

查看 `runLoop` 中的 `timer` 信息发现，它会记录 `timer` 下次要执行的时间，当 `runLoop` 到下一次循环的时候，会检测 `timer` 是否需要执行，这也就是 `timer` 不准的原因，因为每一次 `runloop` 后才会执行 `timer` 的事件。

```objc
{
  valid = Yes,
  firing = No,
  interval = 1,
  tolerance = 0,
  next fire date = 538315424 (-6.348737 @ 113761747014063),
  callout = (NSTimer) [Lefex handlePageStayTimer:],
  context = <CFRunLoopTimer context 0x600000226680>
 }
```

总结：

从上面的问题来看，`NSTimer`对学习 `runLoop` 有很大帮助。`runLoop` 和线程是一一对应的，而除主线程外，其他线程对应的 `runLoop` 并没有创建，当调用 `[NSRunLoop currentRunLoop]` 时会创建这个线程对应的 `runLoop`，定时器能跑起来的前提是 `runLoop` 必须 `run`。


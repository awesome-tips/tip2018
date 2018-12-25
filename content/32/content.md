## Xcode 调试技巧之 attach to process by PID or name

**作者**: [iOS程序犭袁](https://weibo.com/luohanchenyilong?refer_flag=1005055010_)

Apple 在 Xcode9 时为我们带来的无线调试，iPhone 距离成为一块儿无孔的钢块又近了一步。

无线调试，作为真机调试的百搭款，都可以搭配哪些调试技巧呢？

这次介绍的这个，虽然 Xcode5 就已经有了，但，搭配无线调试，绝对有 1+1 > 2 的功效。

场景一：

测试妹子，测试出一个 bug，如何优雅保留现场？迅速定位问题？

连上数据线，`Command+R` 复现一遍？效率太低。

使用该方法，搭配无线调试，直接保留现场，追加断点，当场debug。姿势满分。

场景二：需要反复断点调试涉及前后台切换，重启App、唤起App操作，但又不需要重新编译。

比如调试推送模块时，需要反复测试 `-application:didFinishLaunchingWithOptions:launchOptions:`  方法调用情况。同样适用。

具体用法见[视频](http://t.cn/Ezz4vwk?m=4295962581631192&u=1692391497)


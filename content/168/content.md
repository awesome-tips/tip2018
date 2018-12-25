## 解决Xcode真机无法联调的野路子

**作者**: [高老师很忙](https://weibo.com/517082456)

最近在真机联调的时候，经常遇到 `App installation failed.<Could not write to the device.>` 的错误提示，所下图所示。

![./1.jpg]

网上的解决也是五花八门，比如说删除 `App` 重新 `Run`，`Clean` 等，发现并没有起什么卵用，并且还会占用很多开发时间。想想一个大型项目要重新编译是一件多么可怕的事情。好嘛，重点来了，图中这种错误，有一个简单而又有效的非官方解决方案——随便找一个 `.h` 或者 `.m` 进行一下增删改即可，是不是很神奇，屡试不爽。`<iPhone has denied the launch request>` 这种错误也可以用这种解决方案。


## UIAlertView 与输入框结合使用时的一个坑

**作者**: [Vong_HUST](https://weibo.com/VongLo)

相信 `UIAlertView` 大家应该都很熟悉，但是最近遇到一个坑。

由于历史原因，项目中还在大量使用 UIAlertView。某天测试过来反馈说，评论框字符长度超过最大长度时，点击发送，弹出一个 `alert` 提示，点击确定后，评论框无法在被激活，也就是没法弹出键盘了。很是怪异，`debug` 无果，搜了一下 `stackoverflow`，发现有人遇到过类似的问题，可以点击末尾的参考链接来查看具体详情。

他给出的解决方案就是把这种情况下的 UIAlertView 换成 UIAlertController。试了下这种方式，果然是可行的，由于之前 UIAlertView 是不依赖其它视图层级的，创建后直接 `show` 就可以了，所以很多地方直接写在了非视图控制器类中。在换成 UIAlertController 之后，由于它是继承自 UIViewController 的，所以必须要有 VC 把它 `present` 起来。解决方案也很简单，写一个 UIViewController 的分类获取当前顶部可见的 ViewController，然后在上面 `present` 出 UIAlertController 即可，获取顶部可见 ViewController 的代码随便一搜就可以找到，这边就不贴了。

> PS：UIAlertController 是 iOS8 以后才提供的，不过相信大家也不用适配 iOS8 之前的系统了吧😂。如果还要适配，那就只能做版本区分了。。。


#### 参考

[iOS 9 - Keyboard pops up after UIAlertView dismissed](https://stackoverflow.com/questions/32744209/ios-9-keyboard-pops-up-after-uialertview-dismissed)


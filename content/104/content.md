## UIScrollView 拖拽滑动时收起键盘

**作者**: [KANGZUBIN](https://weibo.com/kangzubin)

当一个页面的 UIScrollView/UITableView 上有一个或多个输入框时，我们经常需要做一件事就是当列表拖拽/滑动时，就收起已经弹起的键盘。

我们工程中之前一直是这么处理的：在 UIScrollView 的 `scrollViewWillBeginDragging` 代理方法（此方法在用户将要开始拖动 scrollView 时调用）中，调用 keyWindow 的 `endEditing:` 方法关闭键盘，代码如下：

```objc
- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView {
    // 开始拖拽滑动时，收起键盘
    [[[UIApplication sharedApplication] keyWindow] endEditing:YES];
}
```

今天发现，原来在 iOS 7 以后，UIScrollView 增加了一个 **keyboardDismissMode** 属性来完成这件事，你只需要写一行代码就可实现上述功能：

```objc
self.tableView.keyboardDismissMode = UIScrollViewKeyboardDismissModeOnDrag;
```

其中，UIScrollViewKeyboardDismissMode 有三个枚举值分别如下：

* UIScrollViewKeyboardDismissModeNone: 默认值，scrollView 的拖拽滑动对键盘状态不会有影响
* UIScrollViewKeyboardDismissModeOnDrag: 当刚拖拽 scrollView 时就会收起关闭键盘
* UIScrollViewKeyboardDismissModeInteractive: 当拖拽 scrollView 向下滑动时，键盘会跟随手势滑动，当又往上滑动时，键盘也会跟着向上并取消关闭。


## 如何使 UIImagePickerController 支持横屏

**作者：** [halohily](https://weibo.com/halohily?refer_flag=1005055010_)

很多同学在开发横屏应用时，使用系统的 `UIImagePickerController` 会发现它默认只支持竖屏。笔者也遇到了这个问题，经过一番探究，如下的方式效果是最佳的：

首先，在 present 这里的 `UIImagePickerController` 对象 picker 之前，设置 picker 的 `modalPresentationStyle` 为 `UIModalPresentationOverCurrentContext`，这时运行会发现横屏时它也可以正常弹出了，只是旋转设备时它不能跟随设备方向正常转动。

接下来，为 `UIImagePickerController` 添加一个 category，重写 `shouldAutorotate` 方法返回 true，重写 `supportedInterfaceOrientations` 方法返回 `UIInterfaceOrientationMaskAll`。这时再运行会发现不仅可以横屏弹出，也可以正常旋转了。


## 使用 AVCaptureSession 踩的一个坑

**作者**: [Vong_HUST](https://weibo.com/VongLo)

最近遇到一个 `bug`，场景是这样的：

应用中有两个相机相关的页面，直播页和直播封面拍摄页（两个页面相机实例不是同一个），进入直播页时，设置了 `preset1（16:9）`，点击进入封面拍摄，会使用 `preset2（4:3）`。如果直播页和封面拍摄页的摄像头位置一致，从封面拍摄页返回到直播页时，会导致相机输出的画面内容变形。后面断点查看，在摄像头位置一致的情况下，从封面拍摄页返回到直播页时，直播页 `AVCaptureSession` 的 `preset` 变为了 `AVCaptureSessionPresetInputPriority` ，而不再是之前的设置的 `preset1`。

通过官方文档以及 `Google` 出的资料，`AVCaptureSessionPresetInputPriority` 代表 `capture session` 不去控制音频与视频输出设置。而是通过已连接的捕获设备的 `activeFormat` 来反过来控制 `capture session` 的输出质量等级。放到我们上面的场景，也就是最终回到直播拍摄页时，`preset` 变成了封面拍摄时的 `preset`（即 `4:3` 的 `preset2` ），进而导致了输出画面变形。其实想想也对，设备全局是同一个摄像头，上一次的更改的参数值，应该是会被保留到下一次的，所以下一次在用之前最好做一次判断是否要重新设值。

解决方案就是每次相机页面 `viewDidAppear` 的时候判断一下 `captureSession` 的 `preset` 和预设的值是否一致，不一致，再设回来即可。

#### 参考：

[在 iOS 上捕获视频](https://objccn.io/issue-23-1/)


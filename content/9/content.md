## 为 UIView “截屏”

**作者：**[halohily](https://weibo.com/halohily?refer_flag=1005055010_)

想为某个 UIView 的内容生成一张图片，系统提供了非常方便的解决方式：使用 CoreGraphics

```objc
UIGraphicsBeginImageContextWithOptions(view.bounds.size, NO, 0);

//获取当前上下文
CGContextRef ctx = UIGraphicsGetCurrentContext();
//渲染
[view.layer renderInContext:ctx];
UIImage *newImage = UIGraphicsGetImageFromCurrentImageContext();

UIGraphicsEndImageContext();

return newImage;
```

若要为 UIView 生成 UIView 形式的快照，可以使用 UIView 的实例方法簇：

```objc
- (nullable UIView *)snapshotViewAfterScreenUpdates:(BOOL)afterUpdates NS_AVAILABLE_IOS(7_0);
```


## “偷窥”第三方APP头文件

**作者**: [Lefe_x](https://weibo.com/u/5953150140)

有时候在开发的过程中需要些黑科技，“偷窥”第三方 APP 使用了哪些第三方库，或猜猜它是如何实现的，咋么办？

其实我们可以使用 `class-dump` 这个工具查看某个 APP 的头文件。只需要找到第三方 APP 的 `xxx.app` 文件，然后执行 `class-dump` 命令即可。不过在执行 `class-dump` 命令前，需要确保 `xxx.app` 是砸过壳的，从 `APPStore` 下载的 `xxx.app` 文件是经过加密处理的，可以直接从各大越狱市场上下载第三方 `xxx.app` 文件，从越狱市场下载的 `xxx.app` 已被破解。可以直接使用 `class-dump` 导出头文件。

### 安装

网上很多教程，不啰嗦了哈。

### 实战

创建一个 `Demo`，然后打包导出 `ipa` 包，找到 `xxx.app`，这里 `xxx.app` 是未经过加密的。具体代码如下：

```objc
@protocol ViewControllerDelegate<NSObject>

- (void)didRefreshDataSuccess;

@end

@interface ViewController : UIViewController

@property (nonatomic, weak) id<ViewControllerDelegate> delegate;
@property (nonatomic, copy) NSString *pubName;

- (void)pubLoadDataWithAlbumID:(NSString *)albumID count:(NSString *)count;

@end

@interface ViewController1 (Navigation)

- (void)setRightBarItemWithTitle:(NSString *)title;

@end
```

执行 `class-dump` 命令：

```ruby
class-dump -H [xxx.app所在的位置] -o [头文件导出的位置]
```

比如：

```ruby
class-dump -H Lefex.app -o lefexheader
```

最终导出的头文件如下：

![](https://github.com/iOS-Tips/iOS-tech-set/blob/master/images/2018/03/9-1.jpg?raw=true)

### 总结

使用 `class-dump` 导出头文件有以下特点：

1. 不管 `.h` 还是 `.m` 文件中的属性和方法都会被导出；
2. 某个类的类别中的方法也会被导出，导出到源文件中，比如 `ViewController (Navigation)` 中的方法被导出到 `ViewController` 中；
3. 实现的协议也会被导出，比如 `ViewControllerDelegate` 的方法被导出到 `ViewController` 中，如果 `ViewController` 不实现 `ViewControllerDelegate` 协议讲不会被导出；
4. 协议中定义的方法不会被导出，只会导出到实现协议的类中；

### 参考

[Class-dump](http://stevenygard.com/projects/class-dump/)

可以参考这个 [@everettjf](https://weibo.com/everettjf?from=usercardnew&refer_flag=0000020001_)  http://everettjf.com/2016/07/09/classify-class-dump-headers-tool/ ，话说可以导出头文件并找出使用的第三方库。不过我试了下没安装成功就没深究，有兴趣的朋友可以看看。


## 宏中的 ## 的含义

**作者**: [Lefe_x](https://weibo.com/u/5953150140)

在宏的定义中，我们也许会遇到过 `##`，比如下面是一些第三方库中 `##` 使用场景：

微信 WCDB 中的宏定义：
`#define __WCDB_BINDING(className) _s_##className##_binding`

唱吧 KTVHTTPCache 定义不同类中是否可以打印的例子：
`#define KTVHCLogEnableValueConsoleLog(target)       KTVHCLog_##target##_ConsoleLogEnable`

那 `##` 有什么用呢？
`##` 在宏中的作用就是先分隔，然后进行强制连接。我们可能会定义不同的函数名或变量时就可以使用这样的宏定义。

那 `##` 是如何工作的呢？

1. `__WCDB_BINDING(className)` ，首先 `_s_##className##_binding` 会拆分成 `_s`,`className `,`_binding `。__WCDB_BINDING(ViewController) 将会被替换成 `_s_ViewController_binding`；

2. `KTVHCLogEnableValueConsoleLog(target)`，首先 `KTVHCLog_##target##_ConsoleLogEnable` 会被拆分为 `KTVHCLog_`, `target ` 和 `_ConsoleLogEnable`。KTVHCLogEnableValueConsoleLog(Lefex) 会被替换成 `KTVHCLog_ Lefex_ConsoleLogEnable`；

3.当使用 KTVHCLogEnable(HTTPServer, YES) ，将会定义一个名为 `KTVHCLog_ HTTPServer_ConsoleLogEnable` 静态常量，初始值为 YES。

```c
#define KTVHCLogEnable(target, console_log_enable)               \
static BOOL const KTVHCLog_##target##_ConsoleLogEnable = console_log_enable;        \
```

比如我们使用不同的 View 名字创建不同的 View：

```objc
#define Name(target) weibo_##target##_name
#define View(target) view##target##Label

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    NSString * Name(lefex) = @"Lefe_x";
    // 打印：You weibo name is: Lefe_x
    NSLog(@"You weibo name is: %@", weibo_lefex_name);
    
    UILabel *View(1) = [UILabel new];
    view1Label.backgroundColor = [UIColor redColor];
    
    UIView *View(2) = [UIView new];
    view2Label.backgroundColor = [UIColor yellowColor];
}
```


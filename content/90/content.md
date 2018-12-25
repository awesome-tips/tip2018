## 是谁调了我的底层库

**作者**: [Lefe_x](https://weibo.com/u/5953150140)

调试的时候，往往底层库会埋一些 `NSLog` 来调试，使用下面这种方式打印出来的函数名称 `__PRETTY_FUNCTION__` 是底层库的函数名。

```c
#   define LEFLog(fmt, ...) NSLog((@"%s (%d) => " fmt), __PRETTY_FUNCTION__, __LINE__, ##__VA_ARGS__)
```

打印是这样的：+[Network post] 中打印了 `I am a log`，而不清楚是谁调用了。

`+[Network post] (22) => I am a log`

但是，我想要的是我在最顶层调用时的函数名，这样我可以很容易的看到是那个方法调用了底层库。

不太理解？举个例子吧：
每个 APP 都会有一个网络层，业务层会直接与网络层进行交互。调试的时候，我想知道 A 请求是在哪个页面中的哪个函数被调用了，咋么办？前提是 `NSLog` 在底层库。我们可以这样实现：

```objc
@implementation LEFLog
+ (NSString *)lastCallMethod
{
    NSArray *symbols = [NSThread callStackSymbols];
    NSInteger maxCount = symbols.count;
    NSString *secondSymbol = maxCount > 2 ? symbols[2] : (maxCount > 1 ? symbols[1] : [symbols firstObject]);
    if (secondSymbol.length == 0) {
        return @"";
    }
    
    NSString *pattern = @"[+-]\\[.{0,}\\]";
    NSError *error;
    NSRegularExpression *express = [NSRegularExpression regularExpressionWithPattern:pattern options:kNilOptions error:&error];
    if (error) {
        NSLog(@"Error: %@", error);
        return @"";
    }
    
    NSTextCheckingResult *checkResult = [[express matchesInString:secondSymbol options:NSMatchingReportCompletion range:NSMakeRange(0, secondSymbol.length)] lastObject];
    NSString *findStr = [secondSymbol substringWithRange:checkResult.range];
    return findStr ?: @"";
}

@end
```

然后定义一个宏：

```c
#   define LEFLog(fmt, ...) NSLog((@"%@, %s (%d) => " fmt), [LEFLog lastCallMethod], __PRETTY_FUNCTION__, __LINE__, ##__VA_ARGS__
```

打印结果是这样的：在 `LefexViewController`  中的 `viewDidLoad` 调用了 `Network ` 的 `post` 方法，并打印 `I am a log`.

```objc
-[LefexViewController viewDidLoad], +[Network post] (22) => I am a log
```


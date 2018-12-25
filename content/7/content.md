## Swift 4.x 中使用 +load 和 +initialize

作者：南峰子

`+load` 和 `+initialize` 方法是我们写 Objective-C 代码时常用的两个方法，不过貌似在 `Swift 4.x` 后，这两个方法在 Swift 类中不那么好使，会报如下编译错误：

```swift
Method 'load()' defines Objective-C class method 'load', which is not permitted by Swift
Method 'initialize()' defines Objective-C class method 'initialize', which is not permitted by Swift
```

所以，如果想在 Swift 类中使用这两个方法，则需要求助于 Objective-C，使用变通的方法，如下代码所示：

```swift
// swift
class Monitor: NSObject {
	@objc class func swiftLoad() {
		// do something
		print("swift load")
	}

	@objc class func swiftInitialize() {
		// do something
		print("swift initialize")
	}
}

// Objective-C
@implementation Monitor (Private)

+ (void)load {
	[self swiftLoad];
}

+ (void)initialize {
	[self swiftInitialize];
}

@end
```

当然，由于这两个方法是 NSObject 类中声明的，所以我们的 Swift 类必须继承自 NSObject 或其子类。另外，我们也可以不用上面这么麻烦地去定义 `swiftLoad/swiftInitialize` 方法，而是所有操作直接在 Objective-C 代码中完成。


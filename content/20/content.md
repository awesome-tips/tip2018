## Swift 4.2 MemoryLayout 新方法 offset(of:)

在 Swift 4.2，`MemoryLayout` 引入是新方法 `offset(of:)`，来计算结构体的某个存储属性的在内存布局上到其所属值的字节距离，以替代 C 语言中的 `offsetof` 函数。

如下示例：

```swift
struct Point {
var x, y: Double
}

struct Size {
var w, h: Double

var area: Double { return w*h }
}

struct Rect {
var origin: Point
var size: Size
}

MemoryLayout<Rect>.offset(of: \.origin.x) // => 0
MemoryLayout<Rect>.offset(of: \.origin.y) // => 8
MemoryLayout<Rect>.offset(of: \.size.w) // => 16
MemoryLayout<Rect>.offset(of: \.size.h) // => 24
MemoryLayout<Rect>.offset(of: \.size.area) // => nil
```

这个方法目前只支持结构体 `struct`，而不支持 `tuple` 和类类型。同时还有不少限制，如只能是存储属性，且不能有 `didSet` 或 `willSet` 等。

具体可以参考 [swift evolution 210：Add an offset(of:) method to MemoryLayout](https://github.com/apple/swift-evolution/blob/master/proposals/0210-key-path-offset.md)


## 使用 CaseIterable 获取枚举的所有 case

作者：南峰子

在写 Swift 代码时，可能需要获取枚举的所有 `case`，在之前的版本中，我们可能需要自己来实现一个静态方法 `allCases`，如下代码所示：

```swift
enum CompassDirection {
    case north, south, east, west

    static let allCases = [north, south, east, west]
}
```

而在 `Swift 4.2` 中，标准库为我们提供了 `CaseIterable` 协议，可以实现这个需求，如下代码所示：

```swift
enum CompassDirection: CaseIterable {
    case north, south, east, west
}

print("There are \(CompassDirection.allCases.count) directions.")
// Prints "There are 4 directions."
let caseList = CompassDirection.allCases
                               .map({ "\($0)" })
                               .joined(separator: ", ")
// caseList == "north, south, east, west"
```

实现 `CaseIterable` 的枚举在编译器会自动合成 `allCases` 方法，且 `allCases` 数组中的元素按 `case` 声明的顺序。不过如果枚举中的关联值或者 `@available` 属性，则需要自己重写 `allCases`，如下代码所示：

```swift
enum CompassDirection: CaseIterable {

    case north, south, east
    case west(Int)

    static var allCases: [CompassDirection] {
        return [north, south, east, west(0)]
    }
}

print("There are \(CompassDirection.allCases.count) directions.")
// Prints "There are 4 directions."
let caseList = CompassDirection.allCases
    .map({ "\($0)" })
    .joined(separator: ", ")
// caseList == "north, south, east, west(0)"
```


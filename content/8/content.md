## SecRandomCopyBytes 生成伪随机数

作者：南峰子

在iOS中，生成伪随机数可以使用这么几个函数：`rand()`、`random()`、`arc4random()`。另外我们知道随机数是密码技术的核心部分，所以 Apple 也为我们提供了相应的生成随机数的方法，即 SecRandomCopyBytes，这个方法位于 `Security.framework` 中，所以使用时需要先导入这个库，使用的方法如下：

```objc
+ (NSString *)generateRandom {

    static int size = 8;
    uint8_t randomBytes[size];
    int result = SecRandomCopyBytes(kSecRandomDefault, size, randomBytes);
    if (result == errSecSuccess) {
        NSMutableString *randomString = [[NSMutableString alloc] initWithCapacity:size * 2];
        for (int i = 0; i < size; i++) {
            [randomString appendFormat:@"%02x", randomBytes[i]];
        }

        return randomString;
    } else {
        return nil;
    }
}
```

这里我们生成一个 8 字节长的 uint8_t 数组，然后将其转换成 hex 字符串得到一个长度16的随机字条串。另外这个函数也可以作为生成 UUID 的辅助操作，如下代码所示：

```objc
+ (NSString*)generateCryptoSecureUUID
{
    unsigned char bytes[16];
    int result = SecRandomCopyBytes(kSecRandomDefault, 16, bytes);
    if (result != noErr) {
        return nil;
    }
    return [[NSUUID alloc] initWithUUIDBytes:bytes].UUIDString;
}
```


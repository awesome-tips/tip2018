## 查看.a静态库中的.o文件及函数接口信息

**作者**: [南峰子_老驴](https://weibo.com/touristdiary)

如果想查看 `.a` 静态库中的 `.o` 文件及函数接口信息，可以尝试下 `nm` 命令。在 `man page` 中查看这个命令的信息，截取一段来看，如下所示：

> Nm displays the name list (symbol table) of each object file in the argument list. If an argument is an archive, a listing for each object file in the archive will be produced. File can be of the form libx.a(x.o), in which case only symbols from that member of the object file are listed. (The parentheses have to be quoted to get by the shell.)  If no file is given, the symbols in a.out are listed.

我们来看看实际效果，以微信的 `libWeChatSDK.a` 为例，使用如下命令：

```ruby
nm -m /Users/**/Desktop/libWeChatSDK.a
```

实际的部分输出如图2所示

```objc
......
(undefined) external _sqlite3_free
(undefined) external _sqlite3_open
(undefined) external _sqlite3_prepare_v2
(undefined) external _sqlite3_reset
(undefined) external _sqlite3_step

/Users/**/Desktop/libWeChatSDK.a(OpenUDID.o):
---------------- (LTO,CODE) non-external +[WXOMTAOpenUDID _generateFreshOpenUDID]
---------------- (LTO,CODE) non-external +[WXOMTAOpenUDID _getDictFromPasteboard:]
---------------- (LTO,CODE) non-external +[WXOMTAOpenUDID _setDict:forPasteboard:]
---------------- (LTO,CODE) non-external +[WXOMTAOpenUDID setOptOut:]
---------------- (LTO,CODE) non-external +[WXOMTAOpenUDID valueWithError:]
---------------- (LTO,CODE) non-external +[WXOMTAOpenUDID value]
(undefined) external _CC_MD5
(undefined) external _CFRelease
(undefined) external _CFStringGetCStringPtr
(undefined) external _CFStringGetFastestEncoding
(undefined) external _CFUUIDCreate
(undefined) external _CFUUIDCreateString
(undefined) external _OBJC_CLASS_$_NSDate
(undefined) external _OBJC_CLASS_$_NSDictionary
......
```

大家可以试试效果。


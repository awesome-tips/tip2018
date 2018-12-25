## 关于 NSDate 中夏令时的坑

**作者：**这个汤圆没有馅

举个例子：

```objc
NSString *birthDay = @"1986-05-04";
NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
[formatter setDateFormat:@"yyyy-MM-dd"];
NSDate *date = [formatter dateFromString: birthDay];
```

你会发现，`1986-05-04` 这个日期拿到的 date 是 null，测几个其他的生日，发现上面这串代码又是 ok 的。经查找资料，发现是因为夏令时引起的原因。关于夏令时的解释，由于篇幅限制这边就不再赘述，可自行百度。夏令时时间段如下：

```
1986年4月13日至9月14日
1987年4月12日至9月13日
1988年4月10日至9月11日
1989年4月16日至9月17日
1990年4月15日至9月16日
1991年4月14日至9月15日
```

经过多次测试，这其中有些日子是可以转化为 NSDate 的。由于没有做覆盖测试，目前只发现了 6 个日期会有问题，可能会有更多。 1986-05-04, 1987-04-12, 1988-04-10, 1989-04-16, 1990-04-15，1991-04-14。

解决方案：

1、使用 `GMT` 零时区

```objc
[formatter setTimeZone:[NSTimeZone timeZoneWithName:@"GMT"]];
```

2、设置 `lenient` 属性

```objc
formatter.lenient = YES;
```

关于 lenient，这个属性没有官方的解释，个人理解为：如果当前时间不存在的话，会默认获取距离最近的整点时间。

以上两种方法，择一即可。


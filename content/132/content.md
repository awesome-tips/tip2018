## iOS9之后字符串变换API

**作者**: [高老师很忙](https://weibo.com/517082456)

今天给大家介绍的是 `iOS9` 之后 `NSString` 新增的方法：

`- (nullable NSString *)stringByApplyingTransform:(NSStringTransform)transform reverse:(BOOL)reverse`
，我用了它的翻译功能之后，喜欢称之为翻译方法，说是翻译可能不太准确，语言变换会更准确一些。

`iOS` 实现了部分 `ICU`（`International Components for Unicode` 的缩写，主要提供提供了 `Unicode` 和全球化支持）功能，所以才有了这个方法的诞生。因此除了系统提供的转换方式（如下图）之外，还支持ICU语法。

![](./1.jpg)

`ICU` 转换语法比如说，简体转繁体（`Hans-Hant`），繁体转简体（`Hant-Hans`），字母过滤（`[:^Letter:] Remove`），还有转换成小写（`Lower`）等等。

我做了一个简单的 `Demo`，核心代码如下：

![](./2.jpg)

![](./3.jpg)

是不是发现项目里汉语转拼音的三方库也可以干掉了？

😜比较遗憾的是 `ICU` 是不支持英文翻译的，所以这个 `API` 也是不支持的。只支持简体，繁体，日语，韩语等多语言的 `APP` 用这个方法还是比较方便的，在处理字符串上面也是比较不错，去掉 `emoji` 也可以一行代码搞定。

`ICU` 相关内容详看：http://userguide.icu-project.org/transforms/general/rules


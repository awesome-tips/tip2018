## iOS12 设置 WKWebView 的 UserAgent

**作者**: [高老师很忙](https://weibo.com/517082456)

最近发现，在 `iOS12` 上通过图1的方式去设置 `WKWebView` 的 `UserAgent` 会失效，

![图1](./1.jpg)

在 `iOS12以下` 这段代码是我们预期的结果（如图2），

![图2](./2.jpg)

在 `iOS12` (16A366)上却发现出现了问题（图3）。

![图3](./3.jpg)

偶然发现如果在设置 `customUserAgent` 之前没有调用过 `webView.evaluateJavaScript(“navigator.userAgent”)` ，设置 `customUserAgent` 是可以设置成功的（图4），

![图4](./4.jpg)

如果在之前调用过（图1，图5），就会悲剧。

![图1](./1.jpg)

![图5](./5.jpg)

如果因为一些特殊原因，需要在调用 `webView.evaluateJavaScript(“navigator.userAgent”) ` 之后设置 `customUserAgent` 的情况，在 `didFinishNavigation` 回调里执行一次 `reload`，在下一次 `didFinishNavigation` 回调之后已经正确设置 `UserAgent`（图6），注意不要循环调用哦！

![图6](./6.jpg)

或者粗暴点，直接延迟几秒调用 `reload`（图7）。

![图7](./7.jpg)

因为最近非常非常忙，所以还没有去深度研究，之后会找时间会深度研究一下，如果有研究过，欢迎一起讨论分享。


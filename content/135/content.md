## 让失去焦点的 UIWebView 弹出键盘的方法

**作者**: [折腾范儿_味精](https://weibo.com/agvicking)

`UIWebView` 无法靠代码的方式让失去焦点的 `webview` 弹出键盘，必须人主动点击 `webview`，代码 `becomefirstresponder` 无效。这个问题是可以通过常规合法手段解决，而不是必须操作私有 `Api`

1 `UIWebView` 有一个属性叫 `keyboardDisplayRequiresUserAction` 默认为 `YES`，就是说只有用户主动操作才可以展现键盘，可以把这行代码置为 `NO`；

2 然后你会发现你调用 `becomefirstresponder` 依然无效，原因是 `iOS Api` 依然无法工作，但是 `keyboardDisplayRequiresUserAction` 关闭，你可以通过 `js` 来操作 `dom` 的 `focus` 事件，从而展现键盘了；

3 `[uiwebview stringByEvaluatingJavaScriptFromString:@"document.getElementById('content').focus()"];`  注入一行 `js` 代码，让 `js` 对页面中的  ‘`content`’ 这个 `dom`，发起 `focus` 事件，键盘弹出；

4 注意，`iOS` 客户端同学不要原样照抄这行 `js` 代码，得根据你的 `web` 页面的 `dom` 甚至三方库自行调整 `js` 的注入代码；

5 简单总结一下：关闭 `keyboardDisplayRequiresUserAction` ，客户端注入 `js`，让 `js` 触发 `input` 区域的 `focus` 事件；


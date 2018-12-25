## iOS 运行时代码注入工具 Injection

**作者**: [halohily](https://weibo.com/halohily)

在 iOS 开发中，对 UI 的代码调整过程中需要反反复复重新运行 build、运行整个项目是一个非常痛苦的过程，尤其在你的项目很大时更是苦不堪言。相信很多 iOS 开发者都羡慕过前端代码的 hot reload，其实有一个工具也可以帮助我们做到 iOS 代码的运行时注入：`Injection`。它最初是一个插件，如今已经有了独立的 Mac 应用程序。它使用 `AppleScript` 与 Xcode 交互，从而确定需要更新的代码文件，一个非常解决痛点的使用场景就是进行 UI 相关的开发时，调整后一键 load 即可查看效果，不再需要重新运行。笔者尝试了一下，确实非常解决问题，有需要的同学可以深入了解。

#### 参考链接

* [Injection III, the App](http://johnholdsworth.com/injection.html)
* [Injection Plugin for Xcode](https://github.com/johnno1962/injectionforxcode)


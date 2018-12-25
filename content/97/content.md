## 查找未国际化的文字

**作者**: [Lefe_x](https://weibo.com/u/5953150140)

对于支持多语言的 App 来说，国际化非常麻烦，而找出项目中未国际化的文字非常耗时（如果单纯的靠手动查找）。虽然可以使用 `Xcode` 自带的工具（`Show not-localized strings`）或者 `Analyze` 找出未国际化的文本，但是它们都不够灵活，而且比较耗时。如果能直接把项目中未国际化的文本导入到一个文件中，直接给产品，然后再使用 **TCZLocalizableTool** `https://github.com/lefex/TCZLocalizableTool`，岂不是事半功倍。下图中就是通过一个 Python 脚本获得的部分未国际化的文本。

![](./1.jpg)

使用很简单

1. 修改 `DESPATH` 路径为你项目的路径
2. 直接在脚本所在的目录下，执行 `python unLocalizable.py`，这里的 `unLocalizable.py` 为脚本文件名。你可以在[这里](https://github.com/lefex/TCZLocalizableTool/blob/master/LocalToos/TCZLocalizable/unLocalizable.py)找到脚本文件。
3. `BLACKDIRLIST` 你可以过滤掉和国际化无关的文件，比如某些第三方库。


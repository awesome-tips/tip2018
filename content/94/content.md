## 删除iOS项目中未使用的图片

**作者**: [Lefe_x](https://weibo.com/u/5953150140)

### 痛点

删除 iOS 项目中没有用到的图片市面上已经有很多种方式，但是我试过几个都不能很好地满足需求，因此使用 Python 写了这个脚本，它可能也不能很好的满足你的需求，因为这种静态查找始终会存在问题，每个人写的代码风格不一，导致匹配字符不一。所以只有掌握了脚本的写法，才能很好的满足自己的需求。如果你的项目中使用 Objective-C ，而且使用纯代码布局，使用这个脚本完全没有问题。当然你可以修改脚本来达到自己的需求。本文主要希望能够帮助更多的读者节省更多时间做一些有意义的工作，避免那些乏味重复的工作。

### 如何使用

1. 修改 `DESPATH` 为你项目的路径；
2. 直接在脚本所在的目录下，打开终端执行 `python unUseImage.py`，这里的`unUseImage.py`为脚本文件名。你可以在[这里](https://github.com/lefex/TCZLocalizableTool/blob/master/LocalToos/unUseImage.py)找到脚本文件；
3. 执行完成后，桌面会出现一个 `unUseImage` 文件夹。文件夹中的 `error.log` 文件记录了可能存在未匹配到图片的文件目录，`image.log` 记录了项目中没使用的图片路径，`images`存放了未使用到的图片。

### 重要提示

当确认 `images` 文件夹中含有正在使用的图时，复制图片名字到`EXCEPT_IMAGES`中，再次执行脚本，确认 `images` 文件夹中不再包含使用的图后，修改`IS_OPEN_AUTO_DEL`为`True`，执行脚本，脚本将自动清除所有未使用的图。

更好的阅读体验，请移步[更人性化的找出 iOS 中未使用的图](http://www.jianshu.com/p/dca77c25bf5d)


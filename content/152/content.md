## 打包时 Xcode 无法及时更新 Provisioning Profile 的解决办法

**作者**: [halohily](https://weibo.com/halohily)

我们在苹果开发者中心新添加一台测试设备的 `UDID` 之后，紧接着打出一份 `Adhoc` 包，却发现新添加的设备还是无法安装。这是因为 `Xcode` 没有及时更新云端的 `Provisioning Profile`。这时可以清除本地目录 `~/Library/MobileDevice/Provisioning` 下的所有内容，然后打包时勾选 `Automatically manage signing` 选项，`Xcode` 会自动下载云端的最新 `Provisioning Profile`。


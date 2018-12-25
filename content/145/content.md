## 为什么音频播放器突然没声音了呢？

**作者**: [Lefe_x](https://weibo.com/u/5953150140)

做音频的同学可能都会遇到播放的音频突然没声音的情况，遇到这种情况后，一般控制台会抛出下面的错误：

```objc
[AVAudioSession setActive:withOptions:error:]: Deactivating an audio session that has running I/O. All I/O should be stopped or paused prior to deactivating the audio session.`
```

遇到这个错误，会导致音频不能正常播放的情况。出现这种情况的主要原因当你设置

```objc
[[AVAudioSession sharedInstance] setActive:NO withOptions:AVAudioSessionSetActiveOptionNotifyOthersOnDeactivation error:nil];
```

时还有某些操作占用了 `AVAudioSession` 权限，必须暂停或停止对 `AVAudioSession` 的使用。比如使用 `AVAudioPlayer` 播放某一音频时，此时音频正在播放，直接设置 `AVAudioSession` 的 `active` 为 `NO`，就会报上面提到的错误。而正确的做法是先暂停播放，再设置 `AVAudioSession` 的 `active` 为 `NO`。其正确的做法像下面代码所示，这样的好处是，当遇到设置失败后可以第一时间知道出错的时间点。

```objc
NSError *error;
BOOL isSuccess = [[AVAudioSession sharedInstance] setActive:active withOptions:AVAudioSessionSetActiveOptionNotifyOthersOnDeactivation error:&error];
if (isSuccess) {
   NSLog(@"恭喜你成功设置");
} else {
   NSLog(@"设置失败");
}
```

当然如果应用中有多个地方使用 `AVAudioSession`，建议项目中统一处理 `AVAudioSession` 的 `active`，这样避免出现错误，一旦出现错误，调试起来就非常费劲。


## iOS 关于音频播放调研

**作者**: [Lefe_x](https://weibo.com/u/5953150140)


由于最近做音频方面的工作，就调研了一下关于音频播放的一些知识，中间也走过不少弯路，希望这篇小集能对关注我们的同学一点启示，少走一些弯路。最后提供一份我看过的资料。这里关于音频播放简单做一个总结。`iOS` 中音频播放有以下 5 种方式（如果你有更多的方式告诉我，非常感激），它们的使用场景各不同。

[1] 播放小于 30s 的音频：
`AudioServicesPlaySystemSound` 可以播放小于等于 `30s` 的音频，主要用于播放一些提示音，你可以利用 `AudioServicesPlaySystemSoundWithCompletion` 的值播放完成的 `callback`。它有以下特点：

- 使用系统音量，不能修改播放音量；
- 立刻开始播放，不能暂停；
- 不支持快进播放，也不可以循环播放；
- 同一时刻只能播放一个音频；
- 只能通过手机播放音频，不能通过其它设备输出，比如不能通过车载播放。

**可在这个链接中查看更多的系统声音ID** `http://iphonedevwiki.net/index.php/AudioServices`

[2] `AVAudioPlayer` 播放本地的音频，或者已加载到内存中的音频流，主要用于播放本地的一些音频文件。注意它不能播放网络音频。它有以下特点：

- 可以从任意位置播放，可快进，快退；
- 可以循环播放；
- 可以同时播放多个音频；
- 可以控制播放速率；

[3] `AVPlayer` 可以播放本地和网络音频，也可以播放视频，它支持流媒体播放，也就是说我们可以用它来做边下别播的使用场景。

[4] `AVQueuePlayer` 是 `AVPlayer` 的子类，它含有一个队列，主要用来播放一个音视频队列。

[5] `Audio Queue` 主要用来播放音频，录音，它比较底层，会有更多的控制权，如果 `APP` 主要功能是基于音频播放，推荐使用这个。

总的来说，如果普通的本地音频播放，可以选择 `AVAudioPlayer` ，这个不需要了解更多的音频知识，就可以达到一个基本的播放；如果想做流媒体播放，建议使用 `AVPlayer + Local Server` 的方式，类似于唱吧目前开源的方式。当然也可以选择 `Audio Queue`，不过这个难度比较高，需要对音频播放有一个整体的了解，推荐使用三方库 `FreeStream`，不过需要一些 `C++` 的知识，因为使用过程中有一些坑需要填，这样不得不阅读源码。最后推荐一些不错的文章。

#### 参考

* [官方 Audio Queue](https://developer.apple.com/library/content/documentation/MusicAudio/Conceptual/AudioQueueProgrammingGuide/AQPlayback/PlayingAudio.html#//apple_ref/doc/uid/TP40005343-CH3-SW1)
* [官方 AudioSession](https://developer.apple.com/library/content/documentation/Audio/Conceptual/AudioSessionProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007875)
* [@pp锅的码农生活 博客](https://brownfeng.github.io/2016/07/25/iOS%E9%9F%B3%E9%A2%91%E7%B3%BB%E5%88%97(%E4%B8%80)/)
* [@cy_zju 博客](http://msching.github.io/blog/2014/07/08/audio-in-ios-2/)


## 使用 Audio Queue 进行流式录音

**作者**: [halohily](https://weibo.com/halohily)

在 iOS 中录音的需求很常见，对于大多数场景采用系统提供的 `AVAudioRecorder`。指定一个音频文件存储路径，即可开启录音。然而，这样我们只能在录音结束之后获得音频数据，无法实时进行。讯飞输入法的 `ASR SDK` 中提供了一个写入音频 data 的接口，就是期望我们自己采用流式录音，录音过程中可以实时获得音频数据，从而接连不断地传递给讯飞的 SDK。

对于流式录音，这里推荐使用 `Audio Queue` 实现。它同样是官方提供的组件，只不过相对于 `AVFoundation` 更底层一些。使用它录音，首先初始化一个音频队列 AudioQueue，然后是三个 `buffer`，用来存储流式录音过程中的每一帧音频 data。注意，通过 `buffer` 大小的不同设置，即可实现每一帧时长的控制。最后，即是实现每一帧完成后的回调函数，在这个函数中完成音频数据的实时传递。

相反的，它同样支持音频的流式播放。除此之外，它还可以满足对音频编码的不同需求。

#### 参考资料

[Audio Queue Services](https://developer.apple.com/documentation/audiotoolbox/audio_queue_services?language=objc)


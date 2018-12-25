## 关于 IAP 丢单的处理

**作者**: [高老师很忙](https://weibo.com/517082456)

做 IAP（In-App Purchase）功能都有可能遇到丢单的问题，丢单是用户已经付款，但是因为某种原因客户端没有办法处理后续的操作，比如说根本没有收到苹果支付成功的回调，或者在与服务器验证票据过程中断网等等。如果处理不好，很容易击溃用户的对产品的信用度。

苹果的推荐做法是合理使用 transaction，在 AppDelegate.m 的 `application:didFinishLaunchingWithOptions:` 方法里添加 `[[SKPaymentQueue defaultQueue] addTransactionObserver:xxx]` ；如果还没有调用 `[[SKPaymentQueue defaultQueue] finishTransaction:xxx]`，那么在你下次启动 App 的时候，就会在 `paymentQueue:updatedTransactions:` 回调里收到未完成的 `transaction`，然后继续进行处理，所以需要你在合适的时机去调用 `finishTransaction` 方法，比如说整个支付流程已经完成（包括已经成功验证票据）的时候，这个可以根据你的业务情况来确定调用时机，这样可以大大降低丢单的概率。不要忘了 `removeTransactionObserver` 哦！

如果觉得处理丢单的时间有点久，可以根据实际情况把相关信息存到本地（如果后续处理流程有需要业务信息的，这种情况是必须要存本地的），在切换到有网或者切换用户或者你觉得合适的实际去处理后续流程，这样也可以双保险，可以把损失降到最低。


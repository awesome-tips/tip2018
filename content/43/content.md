## 一入IAP深似海

**作者**: [高老师很忙](https://weibo.com/517082456)

在做 `IAP` 的时候，我们通常会给 `SKMutablePayment` 对象的 `applicationUsername` 传入一个值，比如说用户ID的哈希值，等交易成功后，通过 `transaction.payment.applicationUsername` 与之前传入的值进行对比校验。

最近发现，交易成功回调返回的 `transaction.payment.applicationUsername` 有可能返回空，并且测试阶段很难发现。解决办法就是增加异常处理逻辑，当 `transaction.payment.applicationUsername` 返回空的时候，要加入一些业务相关的逻辑判断。例如，你的 `applicationUsername` 传入的是用户ID相关的信息，当 `transaction.payment.applicationUsername` 返回为空的时候，就要增加判断发生购买行为和收到回调时是否是同一个用户的逻辑，或者根据你具体的业务而定补充逻辑。


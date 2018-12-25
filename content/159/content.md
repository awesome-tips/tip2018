## 弱持有容器方案

**作者**: [高老师很忙](https://weibo.com/517082456)

在通知者模式中，`manger` 想弱持有一些对象，我们可以怎么做呢？

方案一：使用`Foundation`为大家提供的弱持有容器：`NSHashTable`、`NSMapTable`、`NSPointerArray`，初始化时把`option`设置成`weak`即可；

方案二：可以使用`CFFoundation`的`CFArrayCreateMutable`等来创建容器，被添加的对象引用计数不会加1；

方案三：可以妙用`NSValue`的`valueWithNonretainedObject`方法，被添加的对象不需要服从`NSCopying`协议；

方案四：使用我们通常用的强持有容器，让被添加的对象weak一下，比如使用`__weak`和`block`配合：在`block`中返回 `weak` 对象。


## 一个由曝光统计引起的对 tableView 估计高度的探究

**作者**: [halohily](https://weibo.com/halohily)

最近需要对一个 tableView 视图中所有 cell 的曝光进行统计，每一个 cell 只要获得一次曝光就需要统计一次，也就是当它滑进滑出屏幕时，需要反复统计。很直观地想到，tableView 的 

```objc
- (void)tableView:(UITableView *)tableView willDisplayCell:(UITableViewCell *)cell forRowAtIndexPath:(NSIndexPath *)indexPath 
```

代理方法是完美契合这个需求的，在 cell 每一次曝光时，它都会被适时调用。然而在开发中却发现，每次 tableView 第一屏的内容渲染出来时，该方法的调用次数和第一屏实际展示的内容偏差很大。对其原因进行了反复探究，最终发现是因为没有为 tableView 设置 `estimatedHeight`，这是个会被很多人忽视的属性。在很多关于 tableView 性能优化的讨论中，提前缓存 cell 高度的手段会被反复提到，原因就在于在之前的 iOS 版本中，渲染第一屏之前，系统会将所有 cell 的高度代理方法调用一遍用以确定 tableView 的 `contentSize`，如果这个方法不能迅速地返回的话，便会严重影响性能。然而，在现在的 iOS 版本中，cell 高度的代理方法不再会被提前全部调用，而是在 cell 被展示时，才调用它的高度代理方法获取准确高度值。那么 contentSize 如何计算呢？答案便是刚刚提到的 estimatedHeight。系统根据设定的 estimatedHeight，计算出估计的 contentSize，并且估计出第一屏需要展示的 cell 数量。这个结果也决定了第一屏的 `willDisplay` 代理方法调用次数。如果 estimatedHeight 没有给出的话，便会使用自适应的高度来计算布局，从而导致首屏偏差严重的 willDisplay 调用情况。

值得注意的是，为了让我们方便地设定估计高度，系统给出了 

```objc
- (CGFloat)tableView:(UITableView *)tableView estimatedHeightForRowAtIndexPath:(NSIndexPath *)indexPath 
```

代理方法，用以应对带有多种高度 cell 的 tableView。

以上叙述可能有并不严谨的地方，只是笔者在遇到相关问题后根据现象进行试验、查阅相关文档后得出的结论，提出来这个问题也希望确切了解的同学能够给出准确解答，欢迎大家讨论，非常感谢！


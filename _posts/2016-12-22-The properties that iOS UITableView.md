---
layout: post

title:  iOS-UITableView你需要掌握的属性

date:   2016-12-22 22:22:49

categories: Objective-C

tags: Objective-C
---


- 在现今的开发中，UITableView和UICollectionView算是最最流行的控件了，基本上每一个应用内部都会多次使用到这类控件，它们统一都继承自UIScrollView

- 平时我们都会用它们来展示数据，但是也有很多情况，比如页面上下拖动的时候导航栏透明度改变、一个工具栏在页面向上拖动到一定程度后悬浮到一个位置不动了、再比如简书的个人页面，头像在导航栏上，头像会随着页面的上下拖动变大变小、再比如上拉下拉刷新的实现、再比如点击状态栏，tabbleView如果不是在最顶部，那么它会立即滚动到页面最顶部等等等等，这些效果全部都是通过UIScrollView的一些属性实现的

- 而且这类控件也算是我们开发中最基本的控件了，基本我们天天都需要和它们打交道，所以说，掌握它们的使用不仅仅是对我们最基本的要求，也可以让我们实现很多效果，但是这些都需要建立在对它们一些易混淆属性理解的基础之上

- 本文将以UITableView为例说明下它的一些关键属性，并结合一些属性简单实现下拉刷新

先来看下这篇文章最后结合相关属性讲解的一个小Demo效果
![](http://upload-images.jianshu.io/upload_images/313229-f151c96cfe1feb30.gif?imageMogr2/auto-orient/strip)


### 知识点

1）**contenSize** -- 滚动视图内容的尺寸

- 这个属性是CGSize类型的，它决定了你的滚动视图是否能滚动，能够滚动多远
- 这个属性对于实现我们本文要讲解的下拉刷新是至关重要的，我们需要明确contenSize包含了tabbleview的什么内容，也就是什么内容才算是contenSize的一部分

- 除了UITableviewCell属于contenSize的一部分之外，tabbleview的头部和尾部视图也算是它的contenSize的一部分

- 如果设置了内边距，那么内边距是不算contenSize的一部分的

- 如果给tabbleview添加子控件，这个子控件也不属于contenSize的一部分

- 而且给scrollview内部子控件添加约束时，最头疼的其实就是这个contenSize，它的contenSize需要根据子控件的尺寸以及子控件与scrollview之间的间距计算
![](http://upload-images.jianshu.io/upload_images/313229-4dc7366bb227ca33.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2）**contentOffset** -- 偏移量

- 是一个CGPoint类型的属性，一般而言，上下滑动，我们需要它的contentOffset.y,左右滑动我们需要它的contentOffset.x

- 这是一个滚动视图最最基本的属性，也是最重要的属性，基本上，上述所有效果都是基于它来实现的

- 当tabbleview是在一个导航条下面的话，那么系统会自动将tabbleview内容增加64的顶部内边距，因此它的偏移量y值默认是-64

- 我们可以这样去理解它，以一个上下滚动的tabbleview为例，`contentOffset.y = tabbleview的frame的左上角的y值 - tabbleview的内容的左上角的y值,`而且这个坐标系以tabbleview的内容的左上角为坐标原点
![](http://upload-images.jianshu.io/upload_images/313229-0773c77b82c510bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3） **showsHorizontalScrollIndicator**和**showsVerticalScrollIndicator**

- 是BOOL类型的，决定了是否隐藏水平或者垂直方法滚动条，之所以提及这2个属性是要说一个注意点，比如我们在使用scrollview时，如果我们需要遍历scrollview的子控件数组，我们就需要注意这2个控件，它也算scrollview的子控件
4）**scrollsToTop**

- 一个BOOL类型的属性，可以控制点击状态栏，是否让不在顶部的滚动视图回到最顶部

- 属性默认为YES，也就是系统默认帮我们实现了这个效果

- 需要说明的是如果页面有2个及以上滚动视图的时候，你需要控制哪一个滚动视图支持scrollsToTop，哪些不支持

5）**estimatedHeight** --- 估算高度

- 这里针对tabbleview说一下这个属性，因为tabbleview不需要我们自己计算contensize，所以默认情况下，tabbleview都是先调用heightForRow方法然后再去调用cellForRow方法，而且heightForRow会根据当前tabbleview有多少行就去调用多少次，无论是否cell当前被展示

- 如果我们给了tabbleview一个估算高度，那么它可以减少heightForRow方法的调用频率，延迟计算目前不需要展示cell的高度，需要展示再去计算，这个也算是这个属性的优点

- 它也是有一定缺点的，既然是估算高度，那我们给多少算合适的，其实给多给少都不太好，给少了，它调用heightForRow方法就会多，给少了，虽然调用heightForRow方法少，但是tabbleview计算它的contensize的误差就越大，明显效果就是滚动条的高度很奇怪(跳跃性很大)，所以还是根据cell平均值给一个估算高度
6）**cellForRowAtIndexPath**和**indexPathForSelectedRow**

- `cellForRowAtIndexPath`这个方法根据传入的`indexPath`可以获得显示在`tableView`上的某一行`cell`

- `indexPathForSelectedRow`这个方法可以在系统没有传入indexPath的方法或者自定义的方法中，通过该方法获得被选中的`cell`的`indexPath`，可以得到`section，row，`也是非常实用的

#### 利用偏移量简单实现一下下拉刷新(仿新浪微博)

- 其实一开始说的那些效果实现起来都是不难的，主要是我们需要对这些基本且关键属性非常理解，知己知彼，百战不殆嘛

- 下面的这些代码主要是关于下拉过程中，更新下拉刷新控件文字以及箭头状态的描述，关于发送网络请求，这里只是模拟一下发送，新添加的数据也是假数据

- 关于下拉刷新控件是否作为tabbleview的头部视图，这个我是建议不要这样做的，因为头部视图一般最好是用来作为轮播器或者广告使用，我们一般最好把刷新控件使用addSubview的方法添加到tabbleview上面最好

![](http://upload-images.jianshu.io/upload_images/313229-c5b2d77462518c62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



```swift
/**
 *  停止拖拽,需要在这个方法里面监听停止拖拽的时候偏移量，根据偏移量判断刷新控件是否完全显示，是的话进入刷新，不是的话直接返回
 */

- (void)scrollViewDidEndDragging:(UIScrollView *)scrollView willDecelerate:(BOOL)decelerate
{
    // 如果正在刷新, 直接返回
    if (self.isHeaderRefreshing) return;

    // 当偏移量 <= offsetY时(注意正负值), 刷新控件就完全出现了
    CGFloat offsetY = - (self.tableView.contentInset.top + self.headerBtn.lb_height);
    [self.loadingView stopAnimating];
    self.headerBtn.imageView.hidden = NO;
    if (self.tableView.contentOffset.y <= offsetY) { // 刷新header完全出现了
        // 进入刷新状态
        [self headerBeginRefresh];
    }
}
```


- 还需要在`scrollViewDidScroll`方法中实时根据偏移量控制下拉刷新控件文字以及箭头的状态，这个监控状态的代码这里不贴出来了，放在代码里面有，注释也是很详细的
开始刷新方法实现

```swift
// MARK: 开始刷新
- (void)headerBeginRefresh
{
    //如果当前正在刷新，那么不往下继续执行
    if (self.isHeaderRefreshing) return;
    //否则，进入刷新状态
    self.headerRefreshing = YES;

    //显示菊花
    self.loadingView.alpha = 1.0;
    [self.loadingView startAnimating];

    [self.headerBtn setTitle:@"加载中..." forState:UIControlStateNormal];

    self.headerBtn.imageView.transform = CGAffineTransformIdentity;
    self.headerBtn.imageView.hidden = YES;

    // 显示加载中...，这个时候这个刷新控件是会自己悬浮在导航栏下面，不需要人为拽着不松手
    //这个效果，我们可以通过增大tabbleview的内边距来达到这个效果
    [UIView animateWithDuration:0.25f animations:^{

        //因为刷新控件的y值就是-50，它自己高度也是50，所以只需要让tabbleview内边距向下走50，那么刷新控件就会完全显示了
        UIEdgeInsets inset = self.tableView.contentInset;
        inset.top += self.headerBtn.lb_height;

        self.tableView.contentInset = inset;
    }];
    //由于是模拟发送网络请求，所以延迟1.5秒后再去加载数据
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1.5 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{

        // 发送请求给服务器
        [self loadNewData];
    });
}
```


- **结束刷新方法实现**

```swift
//MARK: 结束刷新
- (void)headerEndRefresh
{
    self.headerRefreshing = NO;
    self.headerBtn.hidden = YES;

    // 减小内边距
    // 刷新已经停止，不需要刷新控件显示在用户能看到的范围，所以需要减少tabbleview的内边距
    [UIView animateWithDuration:0.25 animations:^{

        UIEdgeInsets inset = self.tableView.contentInset;
        inset.top -= self.headerBtn.lb_height;
        self.tableView.contentInset = inset;

    }completion:^(BOOL finished) {//刷新控件缩回到用户看不到的位置后，更新刷新控件以及内部子控件的状态
        self.headerBtn.hidden = NO;
        self.loadingView.alpha = 0.0;
        [self.loadingView stopAnimating];
    }];
}
```
ok，效果实现基本结束了，感谢您的阅读，不足之处欢迎指正!!!
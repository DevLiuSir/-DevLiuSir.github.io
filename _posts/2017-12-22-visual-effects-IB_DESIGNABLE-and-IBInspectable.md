---
layout: post
title: 可视化效果编程 IB_DESIGNABLE 和 IBInspectable 的用法
date: 2017-12-22 22:22:49 + 0900
categories: [能工巧匠集, iOS]
tags: [Objective-C, iOS, Xcode, IB_DESIGNABLE, IBInspectable]
---

我们经常会在用一些自定义 `UIView` 来完成一些特殊的UI效果，但是怎么让我自定义的 `UIView` 在 `Storyboard` 中预览和修改一些自定义参数呢。这就需要用到两个吊吊的东西。

- `IB_DESIGNABLE`: 让你的自定义 `UIView` 可以在` IB(Interface Builder) `中预览。
- `IBInspectable`: 让你的自定义 `UIView` 的属性出现在 IB 中` Attributes inspector` 。


### 环境
- OS X 10.11
- Xcode 7
- Objective-C

## 自定义 UIView

我们先来自定义一个简单的 UIView，叫做` CircleView`，顾名思义，可以显示一个圆形的 `UIView`。

我们想一下 `CircleView` 会有那些属性。

```objc
@interface CircleView : UIView
@property (nonatomic, assign) CGFloat lineWidth;  // 圆形线条的宽度
@property (nonatomic, assign) CGFloat radius;     // 圆形的半径
@property (nonatomic, strong) UIColor *color;     // 绘制的颜色
@property (nonatomic, assign) BOOL fill;          // 是否填充，是不是实心圆
@end
```

有了这些属性我们开始绘制。

```objc
- (void)drawRect:(CGRect)rect {
    // 计算中心点
    CGFloat centerX = (self.bounds.size.width - self.bounds.origin.x) / 2;
    CGFloat centerY = (self.bounds.size.height - self.bounds.origin.y) / 2;
    UIBezierPath *path = [[UIBezierPath alloc] init];
    // 添加一个圆形
    [path addArcWithCenter:CGPointMake(centerX, centerY) radius:_radius startAngle:0 endAngle:360 clockwise:YES];
    // 设置线条宽度
    path.lineWidth = _lineWidth;
    // 设置线条颜色
    [_color setStroke];
    // 绘制线条
    [path stroke];
    if (_fill) {
        // 如果是实心圆，设置填充颜色
        [_color setFill];
        // 填充圆形
        [path fill];
    }
}
```

OK，一个简单的圆形 `UIView` 就搞定了，现在我们想在` IB` 中使用这个 `CircleView`，打开一个 `Storyboard`，拖一个 `UIView` 上去，然后设置 `UIView` 的 `Class` 为我们自定义的 `CircleView`。


![](/assets/images/2017/Visual-Effects-Programming/0.png)

![](/assets/images/2017/Visual-Effects-Programming/1.png)

这时候我们是看不到预览效果的。

![](/assets/images/2017/Visual-Effects-Programming/2.png)

## IB_DESIGNABLE

想要在 IB 中预览，只需要在自定义的 UIView 加上 `IB_DESIGNABLE` 修饰即可。

```objc
IB_DESIGNABLE
@interface CircleView : UIView
...
@end
```

之后再回到 `Storyboard` 中，会重新 `building` 一下，就可以看到预览效果啦。

![](/assets/images/2017/Visual-Effects-Programming/3.png)


当然需要设置 `CircleView` 的几个属性，上面代码中我并有写默认值，所以是什么都看不到的，默认值自己设置一下就好我就不贴代码。能预览了之后，下一步就是怎么在 `Storyboard` 中直接修改 `CircleView` 的属性，半径、颜色之类的。

## IBInspectable

还记得上面我们定义的 `CircleView` 的几个属性吧，只要我们能在 `Storyboard` 中修改这几个属性值，那么 `CircleView` 就会绘制出我们想要的效果。我们只需要属性声明的时候加上 `IBInspectable` 修饰，`Xcode` 会自动添加到 `Storyboard` 中 `Attributes inspector` 栏目中。

```objc
@property (nonatomic, assign) IBInspectable CGFloat lineWidth;
@property (nonatomic, assign) IBInspectable CGFloat radius;
@property (nonatomic, strong) IBInspectable UIColor *color;
@property (nonatomic, assign) IBInspectable BOOL fill;
```


`OK`，再回到 `Storyboard` 中，`building` 之后，选中我们的 `UIView` 元素，在右边切换到 `Attributes inspector` 栏目，就可以在顶部看到我们自定义的属性。

![自定义属性](/assets/images/2017/Visual-Effects-Programming/4.png)


Xcode 会根据不同的数据类型提供不同的输入框，我们可以随意修改属性，就可以看到预览效果。

|Description | Preview |
|----|------|
|![](/assets/images/2017/Visual-Effects-Programming/5.png) | ![](/assets/images/2017/Visual-Effects-Programming/6.png) |
|![](/assets/images/2017/Visual-Effects-Programming/7.png)  | ![](/assets/images/2017/Visual-Effects-Programming/8.png) |


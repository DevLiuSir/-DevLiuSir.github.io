---
layout: post
title:  iOS开发之CAShapeLayer初探
date:   2017-12-29 22:21:49
categories: 能工巧匠集
tags: 能工巧匠集
---



###  一、CAShapeLayer简介

`CAShapeLayer` 属于 `QuartzCore` 框架，继承自 `CALayer`。`CAShapeLayer` 是在坐标系内绘制贝塞尔曲线的，通过绘制贝塞尔曲线，设置shape(形状)的path(路径)，从而绘制各种各样的图形以及不规则图形。因此，使用 `CAShapeLayer` 需要与 `UIBezierPath` 一起使用。

`UIBezierPath`类允许你在自定义的 `View` 中绘制和渲染由直线和曲线组成的路径.。你可以在初始化的时候直接为你的 `UIBezierPath` 指定一个几何图形。
通俗点就是`UIBezierPath`用来指定绘制图形路径，而 `CAShapeLayer` 就是根据路径来绘图的。

### 二、CAShapeLayer属性介绍

#### 1、[CAShapeLayer layer].path

![](https://upload-images.jianshu.io/upload_images/2574378-17d1bef2fe9628ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

```objc
@property(nullable) CGPathRef path;
```

官方的解释是定义要呈现的形状的路径。如果路径扩展到层边界之外，只有当正常层屏蔽规则导致该条线时，它才会自动被剪辑到该层。赋值时，路径被复制。默认为null。（注意，虽然路径属性的动画，不隐将动画时创建的属性发生了变化。）

#### 2、[CAShapeLayer layer].fillColor

![fillColor](https://upload-images.jianshu.io/upload_images/2574378-afca7c6004cc7b19.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


```objc
@property(nullable) CGColorRef fillColor;
```

官方解释是填充路径的颜色，或不需要填充。默认颜色为不透明的黑色。

#### 3、[CAShapeLayer layer].fillRule

![fillRule](https://upload-images.jianshu.io/upload_images/2574378-afd3ea4e20b8fb31.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


```objc
@property(copy) NSString *fillRule;
```

官方解释是当在填充颜色的时候则就需要这种填充规则，值有两种，非零和奇偶数，但默认是非零值。

#### 4、[CAShapeLayer layer].strokeColor

![strokeColor](https://upload-images.jianshu.io/upload_images/2574378-154c82802d4bd827.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

```objc
@property(nullable) CGColorRef strokeColor;
```

官方解释是设置描边色，默认无色。

#### 5、[CAShapeLayer layer].strokeStart与[CAShapeLayer layer].strokeEnd

![strokeStart与strokeEnd](https://upload-images.jianshu.io/upload_images/2574378-7de81b3f03bc80cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

```objc
@property CGFloat strokeStart;
@property CGFloat strokeEnd;
```

官方解释是这两个值被定义用于绘制边线轮廓路径的子区域。该值必须在[0,1]范围，0代表路径的开始，1代表路径的结束。在0和1之间的值沿路径长度进行线性插值。strokestart默认为0，strokeend默认为1。

#### 6、[CAShapeLayer layer]. lineWidth与[CAShapeLayer layer].miterLimit

![lineWidth与miterLimit](https://upload-images.jianshu.io/upload_images/2574378-01c0db7ba24f9a41.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

```objc
@property CGFloat lineWidth;
@property CGFloat miterLimit;
```

官方解释是 `lineWidth`为线的宽度，默认为1；miterLimit为最大斜接长度。斜接长度指的是在两条线交汇处和外交之间的距离。只有lineJoin属性为`kCALineJoinMiter`时miterLimit才有效。边角的角度越小，斜接长度就会越大。为了避免斜接长度过长，我们可以使用miterLimit属性。如果斜接长度超过`miterLimit`的值，边角会以lineJoin的“bevel”即`kCALineJoinBevel`类型来显示。

#### 7、[CAShapeLayer layer]. lineCap与[CAShapeLayer layer].lineJoin

![lineCap与lineJoin](https://upload-images.jianshu.io/upload_images/2574378-4aea159ad16c2559.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

```objc
@property(copy) NSString *lineCap;
@property(copy) NSString *lineJoin;
```

lineCap为线端点类型，值有三个类型，分别为kCALineCapButt 、kCALineCapRound 、kCALineCapSquare，默认值为Butt；lineJoin为线连接类型，其值也有三个类型，分别为kCALineJoinMiter、kCALineJoinRound、kCALineJoinBevel，默认值是Miter。

#### 8、[CAShapeLayer layer]. lineDashPhase与[CAShapeLayer layer]. lineDashPattern

![lineDashPhase与lineDashPattern](https://upload-images.jianshu.io/upload_images/2574378-57cf2fd8721efa64.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

```objc
@property CGFloat lineDashPhase;
@property(nullable, copy) NSArray<NSNumber *> *lineDashPattern;
```

官方解释是lineDashPhase为线型模版的起始位置；lineDashPattern为线性模版，这是一个NSNumber的数组，索引从1开始记，奇数位数值表示实线长度，偶数位数值表示空白长度。
注：fillColor与strokeColor都是在有UIBezierPath参数配置的情况下才能发生作用

### 三、开始绘制

#### 1、颜色说明---矩形为例

![颜色说明](https://upload-images.jianshu.io/upload_images/2574378-5b787c866f643bff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


```objc
CAShapeLayer *layer = [CAShapeLayer layer];
layer.frame = CGRectMake(375/2-50, 667/2-50, 100, 100);
//设置背景色
layer.backgroundColor = [UIColor cyanColor].CGColor;
//设置描边色
layer.strokeColor = [UIColor blackColor].CGColor;
//设置填充色
layer.fillColor = [UIColor redColor].CGColor;
[self.view.layer addSublayer:layer];
```

效果如图显示一个青色的矩形图：

![绘图1](https://upload-images.jianshu.io/upload_images/2574378-c37f720a15f9db2a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/120)

读者估计会纳闷，为啥设置的描边色与填充色没有作用，这是因为这两种颜色需要 `UIBezierPath` 来绘制路径，然后使用 `CAShapeLayer` 进行渲染，所以接下来就就在上面的那段代码下简单的添加一个 `UIBezierPath` 类路径（温馨提示，layer的背景与这两种颜色最好不要共用）。

```objc
UIBezierPath *path = [UIBezierPath bezierPathWithRect:CGRectMake(0, 0, 100, 100)];
layer.path = path.CGPath;
```

![绘图2](https://upload-images.jianshu.io/upload_images/2574378-5bf6329714f99e54.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/120)

可以发现填充色与描边色有效果了。

#### 2、绘制shape

```objc
//绘制矩形
UIBezierPath *path = [UIBezierPath bezierPathWithRect:CGRectMake(0, 0, 100, 100)];
//绘制圆形路径
UIBezierPath *path = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(0, 0, 100, 100)];
//绘制自带圆角的路径
UIBezierPath *path = [UIBezierPath bezierPathWithRoundedRect:CGRectMake(0, 0, 100, 100) cornerRadius:30];
//指定矩形某一个角加圆角（代码示例为左上角）
UIBezierPath *path = [UIBezierPath bezierPathWithRoundedRect:CGRectMake(0, 0, 100, 100) byRoundingCorners:UIRectCornerTopLeft cornerRadii:CGSizeMake(50, 50)];
```

![绘图3](https://upload-images.jianshu.io/upload_images/2574378-6899e4b84494407a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/200)


#### 3、绘制曲线（正弦曲线为例）

先来两张效果图：


![绘图4](https://upload-images.jianshu.io/upload_images/2574378-09d0936b87713b18.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/200)

![绘图5](https://upload-images.jianshu.io/upload_images/2574378-d64b49afd222471f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/200)


说明：图4只是比图5多了填充色而已。


```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    UIBezierPath *path = [self startPoint:CGPointMake(50, 300) endPoint:CGPointMake(200, 300) controlPoint:CGPointMake(125, 200)];
    UIBezierPath *path1 = [self startPoint:CGPointMake(200, 300) endPoint:CGPointMake(350, 300) controlPoint:CGPointMake(275, 400)];
    CAShapeLayer *layer = [self createShapeLayer:[UIColor orangeColor]];
    layer.path = path.CGPath;
    CAShapeLayer *layer1 = [self createShapeLayer:[UIColor greenColor]];
    layer1.path = path1.CGPath;
}

/**
 配置贝塞尔曲线
 @param startPoint 起始点
 @param endPoint 结束点
 @param controlPoint 控制点
 @return UIBezierPath对象
 */
- (UIBezierPath *)startPoint:(CGPoint)startPoint endPoint:(CGPoint)endPoint controlPoint:(CGPoint)controlPoint
{
    UIBezierPath *path = [UIBezierPath bezierPath];
    [path moveToPoint:startPoint];
    [path addQuadCurveToPoint:endPoint controlPoint:controlPoint];
    
    return path;
}
- (CAShapeLayer *)createShapeLayer:(UIColor *)color
{
    CAShapeLayer *layer = [CAShapeLayer layer];
    //    layer.frame = CGRectMake(0, 0, 50, 50);
    //设置背景色
    //    layer.backgroundColor = [UIColor cyanColor].CGColor;
    //设置描边色
    layer.strokeColor = [UIColor blackColor].CGColor;
    //设置填充色
    layer.fillColor = color.CGColor;
    [self.view.layer addSublayer:layer];
    
    return layer;
}
```


#### 4、曲线动画

##### 1)、直线

![直线动画](https://upload-images.jianshu.io/upload_images/2574378-a7bc6c78a8c410e1.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/319)

```objc
UIBezierPath *path = [UIBezierPath bezierPath];
[path moveToPoint:CGPointMake(50, 667/2)];
[path addLineToPoint:CGPointMake(375/2, 667/2)];
[path addLineToPoint:CGPointMake(300, 667/2)];
    
CABasicAnimation *animation = [CABasicAnimation animationWithKeyPath:@"strokeEnd"];
//每次动画的持续时间
animation.duration = 5;
//动画起始位置
animation.fromValue = @(0);
//动画结束位置
animation.toValue = @(1);
//动画重复次数
animation.repeatCount = 100;
    
CAShapeLayer *layer = [self createShapeLayerNoFrame:[UIColor orangeColor]];
layer.path = path.CGPath;
layer.lineWidth = 2.0;
//设置图形的弧度
//    layer.strokeStart = 0;
//    layer.strokeEnd = 0;
 [layer addAnimation:animation forKey:@"strokeEndAnimation"];
//注：由于UIBezierPath已经设置路径，所以动画的路径就不需要再次设置，只需要设置起始0与结束1就行，有需要可以设置动画结束后是否需要返回原位置。
```

##### 2)、曲线

![曲线动画.gif](https://upload-images.jianshu.io/upload_images/2574378-dd2afbade3cc4e5c.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/319)

```objc
UIBezierPath *path = [UIBezierPath bezierPath];
//起始点
[path moveToPoint:CGPointMake(50, 667/2)];
//结束点、两个控制点
[path addCurveToPoint:CGPointMake(330, 667/2) controlPoint1:CGPointMake(125, 200) controlPoint2:CGPointMake(185, 450)];
    
CABasicAnimation *animation = [CABasicAnimation animationWithKeyPath:@"strokeEnd"];
animation.duration = 5;
animation.fromValue = @(0);
animation.toValue = @(1);
animation.repeatCount = 100;
    
CAShapeLayer *layer = [self createShapeLayerNoFrame:[UIColor clearColor]];
layer.path = path.CGPath;
layer.lineWidth = 2.0;
[layer addAnimation:animation forKey:@"strokeEndAnimation"];
```


##### 3)、圆形

笔者目前的一个项目中就用到了这个功能。直接贴一张效果图：

![圆形动画.gif](https://upload-images.jianshu.io/upload_images/2574378-9a8412e965156b88.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/319)


其实实现效果很简单，只是把边线加粗了然后实现动画就可以了，还有一种思路就是画两个圆，截取中间的环，对大圆进行颜色渐变填充，小圆clear所有颜色再去实现动画也能达到这样的效果。

```objc
UIBezierPath *path = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(375/2-100, 667/2-100, 200, 200)];
    
CABasicAnimation *animation = [CABasicAnimation animationWithKeyPath:@"strokeEnd"];
animation.duration = 3.0;
animation.fromValue = @(0);
animation.toValue = @(1);
animation.repeatCount = 100;
    
CAShapeLayer *layer = [self createShapeLayerNoFrame:[UIColor clearColor]];
layer.path = path.CGPath;
layer.lineWidth = 25.0;
//圆的起始位置，默认为0
layer.strokeStart = 0;
//圆的结束位置，默认为1，如果值为0.75，则显示3/4的圆
layer.strokeEnd = 1;
[layer addAnimation:animation forKey:@"strokeEndAnimation"];
```


#### 四、小结

iOS中画图类还有 `CoreGraphics`，但笔者比较喜欢使用`CAShapeLayer`，且`CAShapeLayer` 一般是与 `UIBezierPath` 连用的，如果有动画功能的话，还可以加上`CABasicAnimation`。这篇文章只是对`CAShapeLayer` 和`UIBezierPath`类如何使用和主要功能实现做了简要的说明，还有一些项目中可能经常用到的圆形进度圈、下载圈，或者某些特效的实现，笔者可能在下一篇文章中简析封装步骤与代码实现，有需要的读者多多关注。
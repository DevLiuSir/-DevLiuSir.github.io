---
layout: post
title:  仿iPhone的锁屏滚动动画 
date:   2016-12-08 22:21:49
categories: 能工巧匠集
tags: 能工巧匠集
---

最近工作上没什么项目，今天下午我就尝试模仿了一个iPhone锁屏底部文字的滚动渐变效果，这里用到的技术点：图层`CALayer`，这个大家应该都不会陌生，嗯，现在呢，我拿CALayer的子类`CAGradientLayer`实现一个`iPhone`锁屏文字渐变的动效，总体来讲，其实我觉得主要的难点就是图层的`mask`属性，这个技术点你理解熟悉了，实现渐变动画就容易多了，这里渐变的文字没有借用label，我的处理是把外界传入的字符串添加到图形上下文，这样减少了控制器的负担，也便于我们移植到别的工程。

![](http://upload-images.jianshu.io/upload_images/1488790-5c696b1dda9cc8f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

先附上整体的效果

![](http://upload-images.jianshu.io/upload_images/1488790-3a449feed2630b11.gif?imageMogr2/auto-orient/strip)

这里单独创建一个继承`UIView`的类，类的`.h`声明一个字符串，用来接受外界传入的文字

```objective-c
/** 外界传入的字符 .h文件 */
@property (nonnull, nonatomic, copy) NSString *textStr;
```

## 说下CAGradientLayer属性

- startPoint 渐变开始的点，映射到layer的矩形范围内
- endPoint 渐变结束的点，映射到layer的矩形范围内
- 默认(0,0)在左下角，(1,1)在右上角，默认起始点的值是(0.5, 0)，结束点的值是(0.5, 1)，如图：
![](http://upload-images.jianshu.io/upload_images/1488790-e5921d799a7cd80c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```swift
//设置渐变的开始点、结束点
 _gradientLayer.startPoint = CGPointMake(0.0, 0.5);
 _gradientLayer.endPoint = CGPointMake(1.0, 0.5);
```
 
- `colors` 颜色类 `CGColorRef` 数组 这里需要黑白两种颜色，但整个过程中需要三种渐变色(黑、白、黑)

```swift
//设置渐变过程中点的颜色
 NSArray *colors =@[
                     (id)[UIColor blackColor].CGColor,
                     (id)[UIColor whiteColor].CGColor,
                     (id)[UIColor blackColor].CGColor
                    ];
 _gradientLayer.colors = colors; //colors 渐变中的每一个渐变点
```

- `locations`  范围(1,1), 数组内的对象必须是单调递增的, "locations"对象对应属性`"colors"`中的对象，用来定位渐变颜色出现的位置，"locations"内第一个对象-渐变开始的点，最后一个对象-渐变结束的点

```swift
//设置每个渐变点颜色对应的位置
NSArray *locations = @[@0.2, @0.5, @0.8];
_gradientLayer.locations = locations;
```

## 处理文字

- 添加文字到图形上下文, 这个就简单了，这里我直接贴代码了，如下：

```swift
 NSMutableParagraphStyle *paragraphStyle = [NSMutableParagraphStyle new];
 paragraphStyle.alignment = NSTextAlignmentCenter;

 NSDictionary *attributes = @{
                              NSFontAttributeName:[UIFont fontWithName:@"HelveticaNeue" size:25.0],
                              NSParagraphStyleAttributeName:paragraphStyle,
                              };

 //开启图形上下文
 UIGraphicsBeginImageContextWithOptions(self.frame.size, NO, 0);

 //绘制字符到图形
 [textStr drawInRect:self.bounds withAttributes:attributes];

 //获取图形上下文
 UIImage *image = UIGraphicsGetImageFromCurrentImageContext();

 //结束图形上下文
 UIGraphicsEndImageContext();
```
 
- 这里说下神秘的`mask`

> 首先，`iOS`的控件自带`alpha`的值，但是针对的是整个icon或者ui的透明效果，不能做到自定义的透明或者镂空效果
`mask`含义：属于`CALayer`，但其并不会呈现出来，它是用其`alpha`来控制原`layer`内容的渲染，若`mask`的`alpha`值为1，那原layer就会渲染出来，反之，就不会渲染出来
`mask`原理：默认会显示`mask`层底部的内容，如果渐变层放在`mask`层上，就不会显示了，需将渐变层添加到view的图层上面
mask层工作原理:按照透明度裁剪，只保留非透明部分，文字是非透明的，因此除了文字，其他都被裁剪掉，这样就只会显示文字下面渐变层的内容，相当于留了文字的区域，让渐变层去填充文字的颜色

- 将图像上下文赋值为图层的内容，并将图层设置为透明，然后将这个图层layer作为被裁减的`视图的layer`的`mask`， 这样就可以实现自定义的透明或者镂空效果

```swift
CALayer *layer = [CALayer layer];

 //layer，其尺寸需与所要裁减的视图的尺寸一样
layer.frame = self.bounds;

 //将图像CGImageRef赋值给layer视图
layer.contents = (id)(image.CGImage);

 //一个裁减图形作为其layer的mask 设置透明
layer.backgroundColor = [UIColor clearColor].CGColor;

 //作为mask的layer不能有superLayer或者subLayer
[layer removeFromSuperlayer];

 //将layer作为渐变图layer的mask
self.gradientLayer.mask = layer;
```

- 设置渐变层的大小

```swift
//和视图view等宽高
self.gradientLayer.frame = self.bounds;
最后就是将渐变层添加到视图view的图层上面，添加动画，代码如下：
[self.layer addSublayer:self.gradientLayer];

/**
 *  KeyPath 告诉系统要执行什么样的动画
 */
CABasicAnimation *aniamtion = [CABasicAnimation animationWithKeyPath:@"locations"];

 /**
 *  在整个动画中，第一个黑色原色从0移动到0.75的位置，白色原色从0移动到1的位置，第二个黑色原色从0.25移动到1的位置
 */
aniamtion.fromValue = @[@0.0,@0.0,@0.25];
aniamtion.toValue = @[@0.75,@1.0,@1.0];

aniamtion.duration = 2.5;
aniamtion.repeatCount = MAXFLOAT;
```

## 最后，外界控制器传值

- 外界控制器只需传入想渐变的字符串即可，如下：
![](http://upload-images.jianshu.io/upload_images/1488790-6d7cc5c69f5101da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
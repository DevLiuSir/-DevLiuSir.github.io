---
layout: post
title: macOS开发之自定义NSTabbar
date: 2017-12-31 22:21:49 +0900
categories: [能工巧匠集, macOS]
tags: [swift, Objective-C, macOS开发]
---

### 前言

参考QQ的例子，窗口的按钮的位置可以调整。这篇文章中以QQ为例，实现主界面的切换。

#### 第一部分：优化顶部背景

仔细的观察`QQ`的顶部，有毛玻璃的透明效果，实现方法很简单，就是使用 `NSVisualEffectView`。


![](https://ws2.sinaimg.cn/large/006tNbRwly1fy6kc5x55ej30nb08wjrs.jpg)


然后修改它的风格：

```swift
// 修改毛玻璃的风格
self.effectView.material = NSVisualEffectMaterialLight;
```


![](https://ws3.sinaimg.cn/large/006tNbRwly1fy6kd64bmdj30fl05igmj.jpg)


对比左右两边，效果就明显了。

#### 第二部分：添加切换按钮

- 2-1、自定义TabbarBtn

首先.h文件中声明变量

```objective-c
//是否被选中
@property (nonatomic,assign) BOOL selected;
//两种状态的图片名称
@property (copy,nonatomic) NSString *downImageName;
@property (copy,nonatomic) NSString *normalImageName;
```


然后监听按钮的状态：`mouseDown`、`mouseUp`、`mouseExited`

```objective-c
//防止按钮按下后移出区域，按钮状态不改变的情况
- (void)mouseExited:(NSEvent *)event{
    if (!self.selected){
        self.image = [NSImage imageNamed:self.normalImageName];
    }
}
- (void)mouseDown:(NSEvent *)event{
    self.image = [NSImage imageNamed:self.downImageName];
    //这句必须加，防止按钮的点击事件被覆盖
    [super mouseDown:event];
}
- (void)mouseUp:(NSEvent *)event{
    if (!self.selected){
        self.image = [NSImage imageNamed:self.normalImageName];
    }
    [super mouseUp:event];
}
```

注意：鼠标的移入移除事件不会直接有效，需要加代码：

```objective-c
- (void)drawRect:(NSRect)dirtyRect {
    [super drawRect:dirtyRect];    
    //响应鼠标移入移出事件
    NSTrackingArea *area =[[NSTrackingArea alloc] initWithRect:self.bounds options:NSTrackingMouseEnteredAndExited|NSTrackingActiveInKeyWindow owner:self userInfo:nil];
    [self addTrackingArea:area];
}
```

- 2-2、自定义TabbarView

新建`NSView`，由于功能和iOS中的`Tabbar`类似，暂且命名为`TabbarView`。然后新建`TabbarView.xib`文件并相关联。


![](https://ws4.sinaimg.cn/large/006tNbRwly1fy6kgoxsgwj309o04qq2y.jpg)


添加按钮Gradient Button继承自TabbarBtn


![](https://ws1.sinaimg.cn/large/006tNbRwly1fy6kh4u7j0j306m02l3y9.jpg)


需要修改按钮的属性：去掉`Bordered`的勾选。

然后在init方法中加载控件

```objective-c
- (instancetype)initWithCoder:(NSCoder *)coder{
    self = [super initWithCoder:coder];
    if (self){
        [self setup];
    }
    return self;
}
- (void)setup{
    //从xib中加载控件
    [[NSBundle mainBundle]loadNibNamed:@"TabbarView" owner:self topLevelObjects:nil];
    [self addSubview:self.messageBtn];
    [self addSubview:self.contactBtn];
    [self addSubview:self.moreBtn];
 }
```

设置按钮的属性，并设置出示状态，在`setup`方法中添加

```objective-c
    //图片赋值
    self.messageBtn.normalImageName = @"menu-message-normal";
    self.messageBtn.downImageName = @"menu-message-down";
    self.messageBtn.selected = YES;
    _lastSelectBtn = self.messageBtn;
    self.contactBtn.normalImageName = @"menu-contact-normal";
    self.contactBtn.downImageName = @"menu-contact-down";
    self.moreBtn.normalImageName = @"menu-more-normal";
    self.moreBtn.downImageName = @"menu-more-down";
    //取消按钮高亮状态，去掉点击时的灰色背景
    [self.messageBtn.cell setHighlightsBy:NSNoCellMask];
    [self.contactBtn.cell setHighlightsBy:NSNoCellMask];
    [self.moreBtn.cell setHighlightsBy:NSNoCellMask];
```

其中`_lastSelectBtn`是代表上次选中的按钮，切换状态时使用。

给按钮添加点击事件

```objective-c
- (IBAction)itemButtonClicked:(TabbarBtn*)sender {
    //新旧选中状态切换
    _lastSelectBtn.selected = NO;
    sender.selected = YES;
    _lastSelectBtn = sender;
}
```

- 2-3、把TabbarView添加到ViewController中
拖拽`CustomView`到`view`顶部，并修改他的类`TabbarView`，添加约束居中显示

![](https://ws4.sinaimg.cn/large/006tNbRwly1fy6kjz5tmwj30mw057jrn.jpg)

运行后：

![](https://ws4.sinaimg.cn/large/006tNbRwly1fy6kjjelvdj30rs080weo.jpg)

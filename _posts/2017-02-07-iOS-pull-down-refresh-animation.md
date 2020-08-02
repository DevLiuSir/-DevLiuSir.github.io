---
layout: post
title: iOS下拉刷新动画之箭头旋转方向处理
date: 2017-02-07 10:00:00 +0900
categories: [能工巧匠集, iOS]
tags: [Animations, 动画, iOS, Objective-C]
---

- 在自定义下拉刷新控件,为了让界面更加友好,通常会设置一些动画效果,比如好看、幽默的GIF图片等

- 通常最简单的也起码要有一个文字描述、`ActivityIndicatorView`和箭头了.而在设置箭头旋转时,有个小细节需要处理一下.

代码:

```objc
switch refreshStatus {
	case JSRefreshStatusNormal:         
  		self.descriptionLabel.text = @"继续使劲拉..."
            [UIView animateWithDuration:0.25 animations:^{
                self.leftImageView.transform = CGAffineTransformIdentity;
            }];
            break;
    case JSRefreshStatusPulling:
            self.descriptionLabel.text = @"松手将刷新...";
            [UIView animateWithDuration:0.25 animations:^{
                self.leftImageView.transform = CGAffineTransformMakeRotation(M_PI);
            }]
            break;
    case JSRefreshStatusWillRefresh:
        self.descriptionLabel.text = @"正在刷新中...";
            [self.indicatorView startAnimating];
            break;
        default:
            break;
    }
    
```
    
### 效果图:
![箭头旋转180°默认](/assets/images/2017/Pull-Down-Refresh/clockwise.gif)


解析:

- 当默认状态下,箭头方向朝下↓,当超过某一个临界值后,发生转动直到箭头方向朝上↑,在没有松手的前提下,当表格的偏移量回归到小于临界值后,箭头的方向再次由向上↑恢复到向下↓.

- 细节在于:整个过程的转动是按照一个顺时针方向完成 ⤵️

如果当箭头转动到朝上指向↑时,按照原来转动的方向(逆时针)再转动回来,该如何实现呢?

![动画图解](/assets/images/2017/Pull-Down-Refresh/01.jpg)


如图所示为iOS中的弧度划分状态,假设默认朝下的黑色尖头就为默认状态

`iOS系统中UIView封装的旋转动画,默认顺时针旋转,并遵循一个就近原则`

* 1.当让控件旋转 `M_PI(180°)` 时, 顺时针和逆时针旋转角度相同,所以默认顺时针进行,箭头由下`↓ M_PI_2 (90°)`位置转动到 `M_PI*2(360°)`朝上↑位置

* 2.当让控件旋转角度不足 `M_PI(小于180°)` 时,也就是图中紫色虚线的位置,顺时针旋转的角度<逆时针旋转的角度,这时当箭头从`M_PI_2`位置旋转到这个不足`M_PI`位置时,顺时针旋转角度最小,所以先顺时针旋转,当从不择 `M_PI` 的位置返回原位时,逆时针旋转的角度<顺时针旋转的角度了,所以会逆时针归位,这样实现的效果就是,两次旋转动做都在圆的左侧进行

* 3.当让控件旋转角度大于 `M_PI(大于180°)`时,也就是图中绿色虚线的位置,先按照顺时针来规划一下旋转后的大概位置,当规划完最终的位置后发现,到达最终的位置逆时针旋转的角度要小于顺时针旋转,所以会逆时针转到指定的位置上,当归位时,顺时针完成,这样实现的效果就是,两次旋转动作都在圆的右侧进行

### 所以实现的方式就是:

- 如果想要让两次的旋转都在圆的左侧进行,就让 `旋转的角度` (减去) `一个很小很小很小的`,小到可以忽略的值,比如 `M_PI-0.001`

- 如果想要让两次的旋转都在圆的右侧进行,就让 `旋转的角度` (加上) `一个很小很小很小的`,小到可以忽略的值,比如 `M_PI+0.001`

```objc
self.leftImageView.transform = CGAffineTransformMakeRotation(M_PI-0.001);
```

- M_PI - 0.001效果: 

### 圆左侧旋转处理

![](/assets/images/2017/Pull-Down-Refresh/left.gif)


```objc
self.leftImageView.transform = CGAffineTransformMakeRotation(M_PI+0.001);
```

- M_PI + 0.001效果:

### 圆右侧旋转处理

![](/assets/images/2017/Pull-Down-Refresh/right.gif)
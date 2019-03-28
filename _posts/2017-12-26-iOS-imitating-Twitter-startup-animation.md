---
layout: post
title: Twitter启动动画示例
date: 2017-12-26 12:21:49 +0900
categories: [能工巧匠集, iOS]
tags: [Animations, 动画, iOS, Swift]
---


当第一次打开 twitter 的时候，就被它的启动动画惊艳到了。然而分析一下这个动画其实也不难实现，于是赶紧做一个出来看看。

![](https://github.com/ChinaHackers/TwitterLauncherAnimation/raw/master/Screencast/Screencast.gif)

### 整个动画效果能够拆分为以下几步:

- 布置一个与 `LaunchScreen` 相同的界面
- 让视图中间的 `Logo` 先缩小后放大直至盖满整个屏幕
- `Logo` 在放大过程中逐渐变透明
- `Initial View Controller` 的内容稍微放大后恢复原状

拆分完之后就好办咯，一步步来实现吧~
### 先布个界面
- 由于后面需要让 logo 变透明，我们选择用 mask 来实现。

```swift
let logoLayer = CALayer()
logoLayer.bounds = CGRect(x: 0, y: 0, width: 100, height: 100)
logoLayer.position = view.center
logoLayer.contents = UIImage(named: "logo")?.cgImage
view.layer.mask = logoLayer
```


![](https://ws2.sinaimg.cn/large/006tNbRwly1fyclx4910cj307t0dwdfq.jpg)

好了，小鸟出来了。
但一开始这个 logo 并不是透明的，于是先在其上盖一层白色的 view，并改一下背景颜色吧。

```swift
let shelterView = UIView(frame: view.frame)
shelterView.backgroundColor = .white
view.addSubview(shelterView)
window!.backgroundColor = UIColor(red: 29 / 255.0, green: 161 / 255.0, blue: 242 / 255.0, alpha: 1)
```

![](https://ws4.sinaimg.cn/large/006tNbRwly1fyclz27oktj307t0dwdfp.jpg)

第一步完成。

### 接着做 Logo 的缩小放大吧

这里我们用 `CAKeyframeAnimation`，让这个缩放动作一气呵成。设置好开始时间、持续时间和各个关键帧，最后让它保持动画最后的状态。


```swift
let logoAnimation = CAKeyframeAnimation(keyPath: "bounds")
logoAnimation.beginTime = CACurrentMediaTime() + 1
logoAnimation.duration = 1
logoAnimation.keyTimes = [0, 0.4, 1]
logoAnimation.values = [NSValue(cgRect: CGRect(x: 0, y: 0, width: 100, height: 100)),
                    NSValue(cgRect: CGRect(x: 0, y: 0, width: 85, height: 85)),
                    NSValue(cgRect: CGRect(x: 0, y: 0, width: 4500, height: 4500))]
logoAnimation.timingFunctions = [CAMediaTimingFunction(name: kCAMediaTimingFunctionEaseOut),
                             CAMediaTimingFunction(name: kCAMediaTimingFunctionDefault)]
logoAnimation.isRemovedOnCompletion = false
logoAnimation.fillMode = kCAFillModeForwards
logoLayer.add(logoAnimation, forKey: "zoomAnimation")
```

![](https://ws1.sinaimg.cn/large/006tNbRwly1fyclzksr25g307t0dvalf.gif)

顺道把 Logo 透明也带上
这个算好渐变透明的时间就好。

```swift
UIView.animate(withDuration: 0.3, delay: 1.4, options: .curveLinear, animations: {
    shelterView.alpha = 0
}) { (_) in
    shelterView.removeFromSuperview()
    view.layer.mask = nil
}
```


![](https://ws4.sinaimg.cn/large/006tNbRwly1fycm075cy2g307t0dvb0m.gif)


最后，让初始界面颠一下
道理跟第二步相同，就不说了~


```swift
let mainViewAnimation = CAKeyframeAnimation(keyPath: "transform")
mainViewAnimation.beginTime = CACurrentMediaTime() + 1.1
mainViewAnimation.duration = 0.6
mainViewAnimation.keyTimes = [0, 0.5, 1]
mainViewAnimation.values = [NSValue(caTransform3D: CATransform3DIdentity),
                            NSValue(caTransform3D: CATransform3DScale(CATransform3DIdentity, 1.1, 1.1, 1)),
                            NSValue(caTransform3D: CATransform3DIdentity)]
view.layer.add(mainViewAnimation, forKey: "transformAnimation")
view.layer.transform = CATransform3DIdentity
```
![](https://ws1.sinaimg.cn/large/006tNbRwly1fycm0vmbkfg307t0dv4qp.gif)


### END!!!!

[完整代码点这里](https://github.com/ChinaHackers/TwitterLauncherAnimation)

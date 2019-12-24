---
layout: post
title: CALayer解析之: CAReplicatorLayer
date: 2017-12-26 22:21:49.000000000 +09:00
categories: [能工巧匠集, iOS]
tags: [Animations, 动画, iOS, Swift]
---


## 1.简介

CAReplicatorLayer能够创建出指定个数的子layer的复制layer，并且根据给出的位移、颜色等进行改变。可以用该图层做出很炫酷的动画效果。

## 2.属性

| 属性 |  内容  |
| :------------: | :------------: |
| instanceCount  	|	创建多少复制，默认1 	|
| preservesDepth 	|  	ture为3D图层，false为2D图层	|
| instanceDelay	|	复制的延时，用在动画时 	|
| instanceTransform |	复制子图层时的产生位移，锚点是replicatorLayer的中心点 |
| instanceColor	|	设置复制图层的颜色，默认白色 |
instanceRedOffset	|	设置复制图层相对上一个复制图层红色的偏移量 |
| instanceGreenOffset	|	设置复制图层相对上一个复制图层绿色的偏移量 |
| instanceBlueOffset	|	设置复制图层相对上一个复制图层蓝色的偏移量 |
| instanceAlphaOffset	|	设置复制图层相对上一个复制图层透明度的偏移量 |


## 3.使用


```swift
import UIKit
class ViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        setupLayer()
    }
  func setupLayer() {        
	//创建
        let replicator = CAReplicatorLayer()
        replicator.frame = view.frame
        
        //设置复制图层个数
        replicator.instanceCount = 30
        //复制间隔
        replicator.instanceDelay = CFTimeInterval(1/30.0)
        //一般为false
        replicator.preservesDepth = false
        //图层颜色
        replicator.instanceColor = UIColor.white.cgColor
        
        //偏移量
        replicator.instanceRedOffset = 0
        replicator.instanceGreenOffset = -1
        replicator.instanceBlueOffset = -1
        replicator.instanceAlphaOffset = 0
        
        //角度
        let angle = CGFloat(M_PI*2.0)/30
        replicator.instanceTransform = CATransform3DMakeRotation(angle, 0, 0, 1)
        
        //子图层
        let instanceLayer = CALayer()
        let layerWidth: CGFloat = 10
        let X = view.bounds.midX - layerWidth/2
        let Y = view.bounds.midY - 100;
        instanceLayer.frame = CGRect(x: X, y: Y, width: layerWidth, height: layerWidth*3)
        instanceLayer.backgroundColor = UIColor.red.cgColor
        replicator.addSublayer(instanceLayer)

        //设置动画
        let fadeAnimation = CABasicAnimation(keyPath: "opacity")
        fadeAnimation.fromValue = 1
        fadeAnimation.toValue = 0
        fadeAnimation.duration = 1
        fadeAnimation.repeatCount = Float(Int.max)

        // 设置初始时为透明，并且添加动画
        instanceLayer.opacity = 0
        instanceLayer.add(fadeAnimation, forKey: "FadAnimation")
        view.layer.addSublayer(replicator)
    }
}
```


## 效果：

![](/assets/images/2017/CAReplicatorLayer.gif)
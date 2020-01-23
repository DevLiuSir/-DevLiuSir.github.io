---
layout: post
title:  CALayer解析5：CAGradientLayer
date:   2017-12-27 22:21:49
categories: 能工巧匠集
tags: 能工巧匠集
---


### 1.简介

`CAGradientLayer` 可以很容易的将多种颜色混合到一起，特别适合用来做一个特定的渐变背景。

### 2.属性

| 属性  |	作用 | 
| :------------: | :------------: |
| colors		|	颜色数组，数组内是包含的CGColor |
| locations	|	每种颜色的锚点，如不指定，则均分，值从0-1 |
| startPoint	|	开始的锚点 |
| endPoint	|	结束的锚点 |
| type		|	layer的类型，目前只有axial一个值 |


- `startPoint` 和 `endPoint` 是一个相对的点，没有具体的位置，x表示位于x轴上的位置，最左边为0，最右边为1。y表示位于y有轴上的位置，0表示最上方，1表示底端。

### 3.使用

```swift
import UIKit
class ViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        setupLayer()
    }
    func setupLayer() {
        //创建出layer
        let gradientLayer = CAGradientLayer()
        gradientLayer.frame = view.frame
        //设置颜色数组
        gradientLayer.colors = [cgColorFor(red: 200, green: 0, blue: 0),
                                cgColorFor(red: 255, green: 100, blue: 30),
                                cgColorFor(red: 255, green: 200, blue: 30),
                                cgColorFor(red: 50, green: 200, blue: 0),
                                cgColorFor(red: 30, green: 0, blue: 100),
                                cgColorFor(red: 50, green: 0, blue: 70)]
        //设置锚点
        gradientLayer.startPoint = CGPoint(x: 0, y: 0)
        gradientLayer.endPoint = CGPoint(x: 0, y: 1)
        view.layer.addSublayer(gradientLayer)
    }
    func cgColorFor(red: CGFloat, green: CGFloat, blue: CGFloat) -> CGColor {
        return UIColor(red: red/255.0, green: green/255.0, blue: blue/255.0, alpha: 1).cgColor
    }
}
```


### 最终效果：

<p align="center">
  <img width="250" height="450" src="https://upload-images.jianshu.io/upload_images/3687616-8e12a7acbcf3706d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/314"/>
</p>
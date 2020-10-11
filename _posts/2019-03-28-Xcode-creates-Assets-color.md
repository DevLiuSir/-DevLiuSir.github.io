---
layout: post
title: Xcode 通过Assets.xcassets文件创建颜色集合
date: 2019-03-28 12:00:00 +0900
categories: [能工巧匠集, Swift]
tags: [Xcode, Assets, Color]
---

### Xcode的Assets.xcassets文件不止能够创建图片集合，也可以创建颜色文件集合。

![](/assets/images/2019/xcode-color-set/1.png)


- 1.创建 New Color set文件就可以看到下图内容

![](/assets/images/2019/xcode-color-set/2.png)

- 2.颜色

>2.1.设置名称

>2.2.使用的设备范围

>2.3.选择颜色--里面有系统默认颜色,也可以到其他位置获取

>2.4.设置RGBA值

![](/assets/images/2019/xcode-color-set/3.png)

### 根据自己的需求,选择具体的值.

```swift
self.view.backgroundColor = UIColor(named: "颜色名称")
```
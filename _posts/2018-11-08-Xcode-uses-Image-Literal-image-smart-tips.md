---
layout: post
title: Xcode使用Image Literal实现代码中图片的智能提示
date: 2018-11-08 22:26:49 +0900
categories: [能工巧匠集, Swift]
tags: [swift, Xcode]
---


过去我们在代码中如果要使用一张资源库（Assets.xcassets）里的图片，通常都这么写：

```swift
let image = UIImage(named: "email")
```

但它和上文提到的 UIColor 颜色一样，也存在同样的问题：
在代码中不能直观地看出图片的样子。
同时我们还要记对名字，如果把把 key 写错了，图片就不会显示。

## 一、Image Literal 介绍

　　为了解决上面这个问题，过去我们常常会装一些 `UIImage` 图片资源辅助插件（比如：`KSImageNamed`）。这样在编写代码时，可以实时预览、或选择相应的图片。

　　而从 `Xcode8` 起，编辑器就自带了一个图片辅助功能：`Image Literal`。让我们不在需要再安装额外插件。

## 二、Image Literal 使用

`Image Literal` 提供如下两种使用方式。

### 方式1：
（1）假设我们要初始化一个 `UIImage` 对象，只要输入`“Image Literal”`（会有智能提示），然后回车。

![](/assets/images/2018/Xcode-Image-Literal-01.png)

（2）这时就会出现一个图片选择器小方块。

![](/assets/images/2018/Xcode-Image-Literal-02.png)

（3）我们双击这个图标就可以弹出上下文菜单，显示出所有的资源图片供我们选择。

![](/assets/images/2018/Xcode-Image-Literal-03.png)

（4）选择完毕后那个小方块就会显示出图片的缩略图。

![](/assets/images/2018/Xcode-Image-Literal-04.png)

把这段代码复制出来，其实是长这样的：

```swift
let image = #imageLiteral(resourceName: "email")
```

### 方式2：
（1）我们还可以直接输入图片名字，也会有智能提示和缩略图。

![](/assets/images/2018/Xcode-Image-Literal-05.png)

（2）回车后，最终效果和上面的是一样的。

![](/assets/images/2018/Xcode-Image-Literal-06.png)

---
layout: post
title: SwiftUI 滑动来解锁(slide to unlock)动画解析
date: 2020-06-20 12:00:00 +0900
categories: [能工巧匠集, SwiftUI]
tags: [swift, swiftui, Animations, 动画, iOS]
---


<img src="/assets/images/2020/slide-to-unlock/ScreenShot.png" width="300">


之前在`UIKit`中实现了这样的动画。 [Github源码](https://github.com/DevLiuSir/LCSlideToUnlock)

通常可以在层上使用CoreAnimation，我想知道这在SwiftUI中会如何实现。 [动画](https://swiftui-lab.com/category/animations/)是SwiftUI中对我来说不容易实现的功能之一！

## 动画修饰符 (AnimatableModifier)

我选择将效果实现为 `AnimatableModifier`。该协议既继承自`Animatable`又继承于`ViewModifier`，因此我们同时实现了 `animatableData` 和 `func body（content：Content）-> some View`。

我的想法是，在 `ViewModifier` 的 `body` 函数中获取任何给定的视图，将其覆盖渐变，然后使用mask（）将其约束到内容中：

```swift
LinearGradient(/* ... */).mask(content)
```

然后通过 `animatableData`完成动画处理：它公开了应该进行动画处理的数据。由于我们的动画是无尽的，所以我选择了一个简单的`CGFloat`，它会从0到1不断地增长。该浮点数表示从左向右移动的渐变的基于水平百分比的位置。

## 正确布局

让我们从实现 `body` 方法开始：

```swift
import SwiftUI

struct Shimmer: AnimatableModifier {

    private let gradient: Gradient

    init(sideColor: Color = Color(white: 0.25), 
        middleColor: Color = .white) {
        gradient = Gradient(colors: [sideColor, middleColor, sideColor])
    }

    func body(content: Content) -> some View {
        content
            .overlay(LinearGradient(
                        gradient: gradient,
                        startPoint: .leading,
                        endPoint: .trailing))
            .mask(content)
    }
}

```

内容上有一个叠加层，该叠加层又被内容掩盖了。这样，我们就不需要了，`GeometryReader`因为覆盖层将完全匹配的框架content。我们LinearGradient目前使用的是一个横跨整个可用空间的。然后，此梯度再次被内容掩盖，从而标记出内容的Alpha通道。这可以很好地与文本配合使用：


内容上有一个叠加层，该叠加层又被内容掩盖了。这样，我们不需要`GeometryReader`，因为覆盖层将完全匹配的框架`content`。我们使用了一个`LinearGradient`，它目前跨越了整个可用空间。然后，此梯度再次被内容掩盖，从而标记出内容的Alpha通道。这可以很好地与文本配合使用：


![](/assets/images/2020/slide-to-unlock/full-width-gradient.png)

## 移动渐变
目前，渐变从`.leading`到`.trailing`。让我们用一个`position`以后可以设置动画的变量来修复它：


```swift
struct Shimmer: AnimatableModifier {

    // ...

    @State private var position: CGFloat = 0.25

    func body(content: Content) -> some View {
        content
            .overlay(LinearGradient(
                        gradient: gradient,
                        startPoint: .init(x: position - 0.2, y: 0.5),
                        endPoint: .init(x: position + 0.2, y: 0.5)))
	// ...
    }
}

```

现在将以动画的25％显示动画：

![](/assets/images/2020/slide-to-unlock/25-gradient.png)

挺好的！渐变现在覆盖了区域的40％（`position ` 左侧为20％，右侧为20％）。
不幸的是，在0％和100％处存在一些伪像，其中仍显示了一半的渐变：


![](/assets/images/2020/slide-to-unlock/0-gradient.png)

![](/assets/images/2020/slide-to-unlock/100-gradient.png)

我们可以通过删除动画边缘的 `±0.2`来解决此问题：

- 在 0％ 处，我们要隐藏渐变的右侧
- 在 100％ 处，我们要隐藏渐变的右侧

这是一个简单的线性程序：

```swift
startPoint: .init(x: position - 0.2 * (1 - position), y: 0.5),
endPoint: .init(x: position + 0.2 * position, y: 0.5)))
```


## 动画位置

为了符合`AnimatableModifier`的`Animatable`部分，我们需要将`position`公开为`animatableData`：

```swift
struct Shimmer: AnimatableModifier {

    // ...

    @State private var position: CGFloat = 0
    var animatableData: CGFloat {
        get { position }
        set { position = newValue }
    }

	// ...
}
```


如果我们从0到1进行动画处理，则每帧SwiftUI将被调用，其值范围从0到1。就像数字流一样：在动画制作过程中，它将采用值0.1、0.2、0.3，...

实际的动画需要在`.onAppear`闭包中触发：


```swift
func body(content: Content) -> some View {
    content
        .overlay(/* ... */)
        .mask(content)
        .onAppear {
            withAnimation(Animation
                            .linear(duration: 2)
                            .delay(1)
                            .repeatForever(autoreverses: false)) {
                position = 1
            }
        }
}
```

让我们对此进行剖析：

- 在`.onAppear`中，我们将`position`设置为 1
- 这包装在`.withAnimation`中
- 我们有2秒的线性动画，延迟了1秒
- 重复播放这3秒的动画


在此处使用`.withAnimation`很重要，因为这只会使闭包内部发生的更改动画。如果我们在视图上选择了`.animation`修饰符，则所有其他更改（例如设备旋转时的帧更改）也会被设置为动画。



## 使用

```swift
  // 文字
Text("slide to unlock")
	.fontWeight(.regular)
	.font(.system(size: 20))
	.modifier(Shimmer())                  
```

结果如下所示：

![](/assets/images/2020/slide-to-unlock/slide-to-unlock.gif)

## 总结

>在SwiftUI中为事物设置动画非常好。预览有很大帮助：在开发此预览时，我进行了多个预览，分别显示了动画在各个阶段的进度。

- 但是我喜欢它的主要原因是SwiftUI不会混用隐喻：在UIKit中，当未暴露某些东西（例如遮罩）时，您总是可以使用层API。没有什么可以阻止您从UIView，UIViewController或另一个UIViewController进行此操作。 SwiftUI严格限制了这些交互：从正确包含一致的API开始，这是一个崭新的开始。

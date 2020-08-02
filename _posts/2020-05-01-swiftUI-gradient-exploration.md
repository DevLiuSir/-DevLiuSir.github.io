---
layout: post
title: "SwiftUI 渐变动画(Gradient)探索"
date: 2020-05-01 15:00:00 +0900
categories: [能工巧匠集, SwiftUI]
tags: [swift, swiftui, gradient, Animations, 动画, iOS]
---


本篇文章主要讲解`LinearGradient`，`RadialGradient`，`RoundedRectangle`这3种渐变效果，我会重点讲一下`LinearGradient`，包括线性渐变的原理。


![](/assets/images/2020/swiftui-gradient/09.jpg)

## LinearGradient
- `LinearGradient`称为线性渐变，在SwiftUI中，使用`LinearGradient`非常简单：

```swift
VStack {
    RoundedRectangle(cornerRadius: 10)
        .fill(LinearGradient(gradient: Gradient(colors: [.green, .orange, .blue]),
                             startPoint: .leading,
                             endPoint: .trailing))
    Text("LinearGradient")
}
```


实现线性渐变需要提供3个参数：

- 颜色数组
- 起点
- 终点

我相信大家都能理解线性是什么意思，比如，从0增加到1，所谓的线性就是（0，0.1， 0.2， 0.3 ... 0.9， 1.0），这很好理解，我们使用一个线性函数来计算结果：

![](/assets/images/2020/swiftui-gradient/10.png)

![渐变图](/assets/images/2020/swiftui-gradient/01.jpg)


对于上图中的颜色渐变，颜色的rgb值需要沿着向量AB执行线性函数。在垂直方向(CD, EF, GH)的色值是相同的。

像上图这种情况，渐变的颜色超过了2种，则会分段执行渐变。

在本例中，先由Green渐变到Orange，再由Orange渐变到Blue，这种渐变方式依次规则类推。

渐变的起点和终点决定了渐变向量AB的方向，如果是从左到右，计算过程如下：

![](/assets/images/2020/swiftui-gradient/horizontal.gif)


如果是左上到右下，过程如下：

![](/assets/images/2020/swiftui-gradient/diagonalDown.gif)


在这里就不展示其他的情况了，原理都是一样的。我们再讲一下，线性函数如何计算？

![](/assets/images/2020/swiftui-gradient/04.jpg)



假设我们起点的rgb为(0, 255, 0),终点为(0,0, 255),则rgb各个分量的渐变为；

- r：0 -> 0
- g：255 -> 0
- b：0 -> 255


我们以分量b来做简单的说明，我们只需要确定函数y=a+bx的参数就行了。

- a等于颜色的初始值，在这里等于0
- b等于终点减起点的值，在这里等于255-0
- x等于渐变的进度，大家看上图，这个进度等于IJ/AB

如下图：

![](/assets/images/2020/swiftui-gradient/horizontal-gradient.gif)

代码：

```swift
ZStack(alignment: .leading) {
    RoundedRectangle(cornerRadius: 0)
        .fill(LinearGradient(gradient: .init(colors: [.green, .blue]), startPoint: /*@START_MENU_TOKEN@*/ .leading/*@END_MENU_TOKEN@*/, endPoint: /*@START_MENU_TOKEN@*/ .trailing/*@END_MENU_TOKEN@*/))
        .frame(width: 400, height: 150, alignment: /*@START_MENU_TOKEN@*/ .center/*@END_MENU_TOKEN@*/)

    RoundedRectangle(cornerRadius: 0)
        .modifier(MyAnimationModifier(pct: show ? 1 : 0, colors: colors))
        .frame(width: 20, height: 200)
        .offset(x: show ? 400 : 0, y: 0)
}
```

我们自定义了一个`MyAnimationModifier`，代码如下：

```swift
struct MyAnimationModifier: AnimatableModifier {
    var pct: Double
    let colors: [UIColor]

    var animatableData: Double {
        get {
            pct
        }
        set {
            pct = newValue
        }
    }

    func body(content: Content) -> some View {
        RoundedRectangle(cornerRadius: 0)
            .fill(color(pct))
    }

    func color(_ pct: Double) -> Color {
        let tmpPct = 1.0 / Double(colors.count - 1)
        let index = Int(pct / tmpPct)
        let percent = (pct - Double(index) * tmpPct) / tmpPct

        let color2Index = ((index + 1) >= colors.count) ? (colors.count - 1) : (index + 1)
        return colorMixer(c1: colors[index], c2: colors[color2Index], pct: CGFloat(percent))
    }

    func colorMixer(c1: UIColor, c2: UIColor, pct: CGFloat) -> Color {
        let rgbSpace = CGColorSpaceCreateDeviceRGB()
        guard let cc1 = c1.cgColor.converted(to: rgbSpace, intent: .defaultIntent, options: nil)?.components else {
            return Color(c1)
        }
        guard let cc2 = c2.cgColor.converted(to: rgbSpace, intent: .defaultIntent, options: nil)?.components else {
            return Color(c2)
        }

        let r = cc1[0] + (cc2[0] - cc1[0]) * pct
        let g = cc1[1] + (cc2[1] - cc1[1]) * pct
        let b = cc1[2] + (cc2[2] - cc1[2]) * pct

        return Color(red: Double(r), green: Double(g), blue: Double(b))
    }
}

```

其实，这里用到的颜色混淆算法是不对的，但没关系，其原理都是相似的。作为结果，上图中，实时获取的颜色跟底部的颜色也有一定的偏差。

## RadialGradient
`RadialGradient`称为: 径向渐变。

![](/assets/images/2020/swiftui-gradient/06.jpg)

如上图所示，某条半径的颜色是相同的，颜色渐变的方向为AF，AD， AE等等，渐变的起点和终点需要我们设置。


## AngularGradient
`AngularGradient`称为: 角度渐变。

![](/assets/images/2020/swiftui-gradient/07.jpg)


如上图所示，在向量AB，AC，AD，AE等方向上颜色是相同的，渐变发生在FGH路径上，也就是说，随着角度的变化，颜色作出相应的渐变效果。

## 总结
>非常明显，这3种渐变都依赖渐变函数，通过改变渐变方向和内在的逻辑，可以实现不同的效果。更进一步，我们可以基于这些逻辑，可以实现更丰富的渐变效果。





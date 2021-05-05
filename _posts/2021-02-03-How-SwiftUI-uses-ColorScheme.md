---
layout: post
title: "SwiftUI如何使用 ColorScheme 为浅暗模式配置图像"
date: 2021-02-03 12:00:00 +0900
categories: [能工巧匠集, SwiftUI]
tags: [swift, swiftui, iOS]
---


如果您使用文本和背景的默认颜色，iOS/macOS 会自行处理深色模式文学。但有时，您希望与自己的风格保持一致，以使品牌更具吸引力或在竞争中脱颖而出。调整到暗模式需要更多的工作，但该框架仍然为您提供了许多不同的选项，让您的生活更轻松。

本文将解释如何使用`ColorScheme`环境变量来区分明暗模式，并在图像在暗模式下可见时显示覆盖。

假设您正在处理应用程序的登录屏幕。您决定使用图像作为背景，以使其对用户更具吸引力。但后来你意识到你选择了一张颜色过于鲜艳的照片来适应黑暗模式。解决方案很少。

Xcode 项目资产可以支持不同模式的不同资产。因此，您可以编辑当前图像或找到应用程序将用于暗模式的其他图像。但是，假设您想向项目添加新的（通常是大的）资产。另一种选择是什么？您可以使用`ColorScheme`环境变量检查当前模式并在图像上方显示深色叠加层。

SwiftUI 提供了一个 `Environment` 属性包装器来获取环境变量。其中一个，默认情况下在`colorScheme` 键下可用，是一个 `ColorScheme` 可以存储两个值的枚举，`light` 和 `dark`. 要获得该设置，请添加：

```swift
@Environment(\.colorScheme) private var colorScheme: ColorScheme
```


就像 SwiftUI 中的几乎所有东西一样，您可以通过多种方式实现暗淡效果。我决定使用ZStack并显示一个具有半透明颜色的矩形。

```swift
if colorScheme == .dark {
    ...
}
```


您可以查看完整的代码示例：

```swift
struct BackgroundView: View {
    
    @Environment(\.colorScheme) private var colorScheme: ColorScheme
    
    var body: some View {
        ZStack {
            VStack(alignment: .center, spacing: 10) {
                Text("SwiftUI colorScheme")
                    .foregroundColor(colorScheme == .dark ? Color.white : .black)
                    .font(.title)
                    .bold()
                Image(systemName: colorScheme == .dark ? "printer.fill" : "printer")
                    .resizable()
                    .scaledToFit()
                    .frame(width: 100, height: 100, alignment: .center)
            }
            .frame(width: 400, height: 400)
        }
        .background(Color.purple)
    }
}
```


效果。如您所见，Text 和 Image 颜色已自动调整。

| **light** | **dark** | 
| :------------: | :------------: |
| <img align="center" src="/assets/images/2020/SwiftUI-ColorScheme/ColorScheme_Light.png" width=180 height=300> | <img align="center" src="/assets/images/2020/SwiftUI-ColorScheme/ColorScheme_Dark.png" width=180 height=300> |
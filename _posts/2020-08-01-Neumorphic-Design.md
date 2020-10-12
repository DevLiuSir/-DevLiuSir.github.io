---
layout: post
title: SwiftUI 之神经形态设计
date: 2020-08-01 12:00:00 +0900
categories: [能工巧匠集, SwiftUI]
tags: [swift, swiftui, iOS]
---

![](/assets/images/2020/swiftui-neumorphism/NeumorphismHeader.png)

### 在本教程中，我们将讨论在您的SwiftUI应用程序中实现Neumorphism接口设计。您将了解2020年用户界面设计的最大趋势以及如何将其用于SwiftUI应用程序。

- `Neumorphic Design`是`Skeomorphic`设计的一种新形式,最初在iPhone推出时就使用过。

- 在本教程的最后，您将能够在SwiftUI应用中创建如下的`Neumorphism`视图：


> 亚同形设计示例（深色外观）

### 什么是同态？ 🔦

首先，什么是同态设计？神经形态（也称为软件设计）是当前平面设计方法和拟形原理的结合。 

在移动应用程序开发的早期，Skeuomorphic Design确实很受欢迎。拟态主义基本上意味着视觉元素的设计应模仿现实生活中的对应元素。

- 与此相反，平面设计遵循一种更简约的方法。尽可能避免对象的逼真的三维表示。


![](/assets/images/2020/swiftui-neumorphism/calculator.jpg)


iOS计算器应用程序的演进是比较拟态设计和平面设计的一个很好的例子
同态（也称为软件设计）结合了这两个原理。Neumorphism也遵循一种极简主义的方法，但是当平面设计的元素通过在其上投射一个简单的阴影在“感知”的背景上“浮动”时，Neomorphic元素似乎已从背景中“剔除”。这将创建一个平滑的3D外观，使我们想起拟态时代。


![](/assets/images/2020/swiftui-neumorphism/modernvsneo.jpg)


我们如何在SwiftUI中实现这种同态效果？首先，元素本身必须具有与其背景相同的颜色。请注意，背景和元素都不应该是纯白色或黑色。接下来，您需要在左上角投射一个光影。最后，您从右下方投射了一个暗影。


![](/assets/images/2020/swiftui-neumorphism/analysis.jpg)

### 创建颜色集 🎨

要在SwiftUI中实现Neumorphism方法，我们建议您首先创建相应的颜色集。为此，请转到Xcode项目的Assets目录，然后右键单击以创建新的颜色集。

您必须总共创建三个颜色集。一种用于背景/元素，一种用于浅色阴影，另一种用于深色阴影。

确保为每种颜色设置创建深色模式的替代颜色。

![](/assets/images/2020/swiftui-neumorphism/color-set.jpg)

> 确保您还为每种颜色集的深色外观创建了替代颜色


以下是可用于“同态设计”的十六进制颜色的建议：

灯光模式颜色：

- 背景/元素：＃DEEAF6
- 阴影：＃F3F9FF
- 暗影：＃BECBD8

深色模式颜色：

- 背景/元素：＃1A1B1E
- 光影：＃242529
- 暗影：＃151518

> 提示：您可以输入HEX值，方法是单击颜色集中的特定颜色，然后单击属性检查器中的“显示颜色面板”。

![](/assets/images/2020/swiftui-neumorphism/image-HEX.jpg)

完成创建所有三种颜色集后，继续创建新变形的SwitUI视图。

### 创建中立的SwiftUI视图 💡
在您的SwiftUI ContentView中，通过创建一个覆盖整个屏幕的Rectangle开始创建彩色背景，如下所示：

```swift
struct Comparison: View {
    var body: some View {
        ZStack {
            Rectangle()
                .fill(Color("Background"))
                .frame(maxWidth: .infinity, maxHeight: .infinity)
                .edgesIgnoringSafeArea(.all)
        }
    }
}
```

然后，创建您的元素，并为其赋予与背景相同的颜色。在这个例子中，我们使用`RoundedRectangle`创建一个简单的卡片。

```swift
ZStack {
   Rectangle()
   		.fill(Color("Background"))
       .frame(maxWidth: .infinity, maxHeight: .infinity)
       .edgesIgnoringSafeArea(.all)
   RoundedRectangle(cornerRadius: 20)
       .fill(Color("Background"))
       .frame(width: 300, height: 180)
}
        
```

最后，通过使用具有不同颜色和偏移位置的.shadow修改器来应用明暗阴影。

```swift
RoundedRectangle(cornerRadius: 20)
                .fill(Color("Background"))
                .frame(width: 300, height: 180)
                .shadow(color: Color("LightShadow"), radius: 8, x: -8, y: -8)
                .shadow(color: Color("DarkShadow"), radius: 8, x: 8, y: 8)

```


### 预览：

![](/assets/images/2020/swiftui-neumorphism/Screenshot.png)

### 结论 🎊
就这样！您可以将本教程中使用的技术应用于您的SwiftUI应用程序中的每个元素，从而创建出类似神经的外观！
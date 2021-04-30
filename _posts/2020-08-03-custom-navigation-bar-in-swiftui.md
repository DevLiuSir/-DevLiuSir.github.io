---
layout: post
title: "SwiftUI 自定义导航栏"
date: 2020-08-03 12:00:00 +0900
categories: [能工巧匠集, SwiftUI]
tags: [swift, swiftui, navigationView, iOS]
---

- 在iOS 14中，SwiftUI提供了一种使用新 `toolbar` 修饰符来自定义导航栏标题视图的方法。这与`navigationItem.titleView` UIKit中的设置相同。

### 一、导航栏标题视图
- 要在SwiftUI中自定义导航栏标题视图，我们只需将`ToolbarItem`展示位置类型设置为`.principal`新的`toolbar`修饰符即可。

```swift
NavigationView { 	// 1
    Text("Hello, SwiftUI!")
        .navigationBarTitleDisplayMode(.inline)
        .toolbar {	// 2
            ToolbarItem(placement: .principal) { 	// 3
                VStack {
                    Text("Title").font(.headline)
                    Text("Subtitle").font(.subheadline)
                }
            }
        }
}
```

1. 由于这是导航栏标题的自定义，因此需要将视图嵌入到中`NavigationView`。
2. 将`.toolbar`修改器设置为的根视图`NavigationView`。
3. 一组`ToolbarItem`放置类型`.principal`，其内容要显示为标题视图。

上面的代码将在SwiftUI中生成此代码。

![](/assets/images/2020/swiftui-custom-navigation-bar/titleview-vstack.png)

这不仅限于文字；您可以在标题视图中使用图像或按钮。

以下示例在标题视图中使用SF符号。

```swift
NavigationView {
    Text("Hello, SwiftUI!")
        .navigationBarTitleDisplayMode(.inline)
        .toolbar {
            ToolbarItem(placement: .principal) {
                HStack {
                    Image(systemName: "sun.min.fill")
                    Text("Title").font(.headline)
                }
            }
        }
}
```


![带有SF符号的自定义标题视图](/assets/images/2020/swiftui-custom-navigation-bar/titleview-hstack.png)

以下示例在标题视图中使用按钮。

```swift
NavigationView {
    Text("Hello, SwiftUI!")
        .navigationBarTitleDisplayMode(.inline)
        .toolbar {
            ToolbarItem(placement: .principal) {
                VStack {
                    Text("Title").font(.headline)
                    Button("Subtitle") {
                        
                    }
                }
            }
        }
}
```


![带有按钮的自定义标题视图](/assets/images/2020/swiftui-custom-navigation-bar/titleview-button.png)


### 二、导航栏元素颜色
SwiftUI的默认导航栏为白色，带有黑色文本，而启用黑暗模式时则相反。

![](/assets/images/2020/swiftui-custom-navigation-bar/normal.png)

在SwiftUI中，目前没有自定义顶部导航栏的简单方法（即更改导航栏的颜色）。但是，SwiftUI确实支持创建自定义视图修饰符的功能，这些修饰符使我们可以根据需要修改或设置视图样式。一些内置的调节剂包括`font()`，`navigationBarTitle()`，和`foregroundColor()`，等等。
在本文中，我将向您展示如何创建一个自定义`ViewModifier`，可用于轻松自定义自定义导航栏`NavigationView`。要创建修饰符，您首先需要定义一个`struct`符合`ViewModifier`协议的。让我们定义一个`NavigationBarColor struct`：


```swift
struct NavigationBarColor: ViewModifier {

  init(backgroundColor: UIColor, tintColor: UIColor) {
    let coloredAppearance = UINavigationBarAppearance()
    coloredAppearance.configureWithOpaqueBackground()
    coloredAppearance.backgroundColor = backgroundColor
    coloredAppearance.titleTextAttributes = [.foregroundColor: tintColor]
    coloredAppearance.largeTitleTextAttributes = [.foregroundColor: tintColor]
                   
    UINavigationBar.appearance().standardAppearance = coloredAppearance
    UINavigationBar.appearance().scrollEdgeAppearance = coloredAppearance
    UINavigationBar.appearance().compactAppearance = coloredAppearance
    UINavigationBar.appearance().tintColor = tintColor
  }

  func body(content: Content) -> some View {
    content
  }
}
```


我们`ViewModifier`使用`backgroundColor`和初始化`tintColor`，前者将用作导航栏的颜色，后者将用作导航栏的文本颜色。这种定制是通过init我们的修饰符的方法完成的。该`body`方法将按原样返回内容。
要使用我们的修饰符，我们可以创建一个扩展View，以应用上面定义的修饰符，如下所示：

```swift
extension View {
  func navigationBarColor(backgroundColor: UIColor, tintColor: UIColor) -> some View {
    self.modifier(NavigationBarColor(backgroundColor: backgroundColor, tintColor: tintColor))
  }
}
```

现在，我们可以在应用程序中的任何位置轻松使用此修饰符。这是一个例子：

```swift
import SwiftUI

struct ContentView: View {
  var body: some View {
    NavigationView {
      List {
        ForEach(0..<25, id: \.self, content: { i in
          NavigationLink("(\(i)) Hello, world!", destination: EmptyView())
        })
      }
      .navigationBarTitle("Bar Title")
    }
    .navigationBarColor(backgroundColor: .systemTeal, tintColor: .white)
  }
}
```

现在，如果您运行应用程序，则可以看到自定义的导航栏。
![自定义SwiftUI导航栏](/assets/images/2020/swiftui-custom-navigation-bar/titleBar.png)

导航到其他视图时，导航栏样式仍然保持：

![自定义导航栏](/assets/images/2020/swiftui-custom-navigation-bar/preview.gif)

###总结：
- 这只是我们如何在SwiftUI中使用视图修饰符的一个示例；修饰符有助于在整个应用中定义一致且可重复使用的样式。我希望本文可以帮助您创建其他有用的修饰符。
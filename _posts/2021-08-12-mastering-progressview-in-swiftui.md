---
layout: post
title: "掌握 SwiftUI 中的 ProgressView"
date: 2021-08-12 12:00:00 +0900
categories: [能工巧匠集, SwiftUI]
tags: [swift, swiftui, iOS]
---

我们的许多应用程序在网络或数据处理等后台线程上做了大量工作。我们通常希望显示正在进行的工作的进度或活动指示器。本周我们将学习如何使用`ProgressView`在 SwiftUI 中呈现不确定和确定的进度。

#### 不确定的进展
显示不确定进度所需要做的就是将`ProgressView`放置在布局中的任何位置。让我们尝试在一个简单的例子中做到这一点。

```swift
struct ContentView: View {
    var body: some View {
        ProgressView()
    }
}
```

![progress](/assets/images/2021/progress/progress2.png)


```swift
struct ContentView: View {
    var body: some View {
        ProgressView("Loading")
    }
}
```


![progress](/assets/images/2021/progress/progress.png)



正如你在上面的例子中看到的，普通的`ProgressView`默认显示一个不确定的圆形指示器。通常，这意味着后台正在进行工作，用户应该等待查看一些结果。我们还可以通过将其放置在圆形活动指示器附近来提供可本地化的字符串。

SwiftUI 默认使用循环活动指示器来显示不确定的进度，但是有一种方法可以使用线性进度指示器来显示确定的进度。


#### 确定进度
`ProgressView`为我们提供了一个特殊的初始化程序，允许我们显示确定的进度。我们可能会在我们预计最终结果一段时间后使用它。例如，我们通常在下载文件时知道文件的最终大小。在这种情况下，我们必须显示出确定的进展。让我们看看如何做到这一点。


```swift
struct ContentView: View {
    var body: some View {
        ProgressView(value: 250, total: 1000)
    }
}
```


![progress](/assets/images/2021/progress/progress1.png)


如您在上面的示例中所见，我们使用`ProgressView`的另一个初始化程序来显示当前进度和总值。SwiftUI 会自动计算完成工作的百分比，并使用线性指示器显示相应的进度。


#### 样式
我们可以使用另一个初始化程序轻松自定义`ProgressView` ，该初始化程序接受代表视图标签的`@ViewBuilder` 。



```swift
struct ContentView: View {
    var body: some View {
        ProgressView {
            Text("Loading")
                .font(.title)
        }
    }
}
```

![progress](/assets/images/2021/progress/progress3.png)

还有一个带有`@ViewBuilder`的初始化程序，它可以确定进度并允许您自定义标签。

像往常一样，SwiftUI 提供了一个样式协议，允许我们完全重新设计默认的外观和感觉。您需要创建一个符合`ProgressViewStyle`的类型并实现 `makeBody` 函数。



```swift
struct HorizontalProgressViewStyle: ProgressViewStyle {
    func makeBody(configuration: Configuration) -> some View {
        HStack(spacing: 8) {
            ProgressView()
                .progressViewStyle(.circular)
            configuration.label
        }.foregroundColor(.secondary)
    }
}

extension ProgressViewStyle where Self == HorizontalProgressViewStyle {
    static var horizontal: HorizontalProgressViewStyle { .init() }
}

struct ContentView: View {
    var body: some View {
        ProgressView("Loading")
            .progressViewStyle(.horizontal)
    }
}
```

在上面的示例中，我们创建了一个水平样式来显示圆形动画指示器和标签。`Configuration` 参数为我们提供了进度的完成部分，它允许我们绘制一个超级自定义的进度视图。



```swift
struct CustomProgressView: View {
    let value: Double

    var body: some View {
        // Draw progress here
    }
}

struct CustomProgressViewStyle: ProgressViewStyle {
    func makeBody(configuration: Configuration) -> some View {
        VStack {
            if let value = configuration.fractionCompleted {
                CustomProgressView(value: value)
            }
            Text("Loading...")
        }
    }
}
```

在这里，我们有一个呈现进度的自定义视图。它可能是您想要的任何东西，从简单的文本到自定义的动画路径。请记住，当您使用不确定的进度视图时， `fractionCompleted`可能为零。


#### 结论
`ProgressView`可能看起来非常简单，但它提供了出色的自定义和样式选项。
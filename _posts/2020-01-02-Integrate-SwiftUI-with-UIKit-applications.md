---
layout: post
title: 将SwiftUI与UIKit应用程序集成以实现早期采用
date: 2020-01-02 14:00:00 +0900
categories: [能工巧匠集, SwiftUI]
tags: [iOS, Swift, swiftui, UIKit]
---


- SwiftUI是在iOS 13中引入的，当时我们许多人都使用UIKit构建了一个大型应用程序。SwiftUI使我们充满热情，就像当Swift被引入以开始使用此用于构建应用程序的新框架一样。

- 引入Swift时，我们不得不从Objective-C迁移到Swift。许多应用程序很可能仍在使用（部分）Objective-C，因为过渡可能会花费一些时间。很高兴知道如何开始过渡以使此过程尽可能短。iOS 14已经接近尾声，您可以考虑在今年晚些时候删除iOS 12，这使您可以在SwiftUI中编写新视图。因此，有一篇博客文章解释了如何在UIKit应用程序中使用SwiftUI。

> 尽早在UIKit应用程序中添加SwiftUI视图使您未来的生活更快乐。

## 在UIKit应用程序中尽快使用SwiftUI的好处
从UIKit应用程序过渡到SwiftUI非常耗时，大型应用程序可能需要花费数年的时间。在引入Swift时，用Objective-C编写的应用程序必须进行相同的转换，并且可能仍未完全移开。因此，尽早采用SwiftUI有一些好处：

- 将来无需将新视图重写为SwiftUI
- 您像平时一样开始学习SwiftUI
- 过渡到SwiftUI需要时间。开始时间越早，您的应用程序将越早用SwiftUI完全编写

## 在UIKit应用程序中采用SwiftUI时要考虑的事项
重要的是要知道您只能在iOS 13及更高版本上使用SwiftUI。最重要的是，SwiftUI是一项新技术，在以后的更新中得到了改进。删除iOS 12意味着您可以从SwiftUI开始，但这也意味着您开始使用SwiftUI的第一个版本。从早期开始，这可能会导致意外行为和错误。

### 将SwiftUI仅用于新功能
关于此事，您可以决定仅将SwiftUI用于新功能。即使您未删除iOS 12，也可能出现这种情况。您将使用可用性API，使新编写的视图仅在支持SwiftUI的版本上可用。

```swift
if #available(iOS 13.0, *) {
    presentSwiftUIView()
} else {
    // Fallback on earlier versions
}
```

您可以决定使用SwiftUI交付新功能，这使该代码具有前瞻性，因为它不需要几年就可以重写它。这显然只有在您可以决定使某个功能仅对iOS 13及更高版本的用户可用时才有效。

## 在UIKit视图控制器中呈现SwiftUI视图
一旦拥有了要使用SwiftUI开发的功能，就可以使用SwiftUI框架进行集成了。SwiftUI引入了一个名为的实例`UIHostingController`，该实例负责“托管” SwiftUI视图。

`UIHostingController`本身继承自`UIViewController`, 它就可以像使用其他任何视图控制器实例一样使用它。因此，呈现SwiftUI视图的代码如下所示：

```swift
func presentSwiftUIView() {
    let swiftUIView = SwiftUIView()
    let hostingController = UIHostingController(rootView: swiftUIView)
    present(hostingController, animated: true, completion: nil)
}
```

## 将SwiftUI视图添加到UIKit视图
相同的技术适用于将SwiftUI视图添加到UIKit视图层次结构。唯一的区别是您不是呈现视图，而是将其添加为子视图控制器：

```swift
func addSwiftUIView() {
    let swiftUIView = SwiftUIView()
    let hostingController = UIHostingController(rootView: swiftUIView)

    /// Add as a child of the current view controller.
    addChild(hostingController)

    /// Add the SwiftUI view to the view controller view hierarchy.
    view.addSubview(hostingController.view)

    /// Setup the constraints to update the SwiftUI view boundaries.
    hostingController.view.translatesAutoresizingMaskIntoConstraints = false
    let constraints = [
        hostingController.view.topAnchor.constraint(equalTo: view.topAnchor),
        hostingController.view.leftAnchor.constraint(equalTo: view.leftAnchor),
        view.bottomAnchor.constraint(equalTo: hostingController.view.bottomAnchor),
        view.rightAnchor.constraint(equalTo: hostingController.view.rightAnchor)
    ]

    NSLayoutConstraint.activate(constraints)

    /// Notify the hosting controller that it has been moved to the current view controller.
    hostingController.didMove(toParent: self)
}
```


我们在这里做一些事情：

- 首先，我们将托管控制器作为子级添加到当前视图控制器中
- 该视图已添加到当前视图控制器的视图层次结构中
- 在代码中设置了约束，以更新SwiftUI视图的边界
- 通知主机控制器其包含的SwiftUI视图已移至父级

## 创建扩展以简化添加SwiftUI视图
为了提高效率，我们可以将此逻辑包装在的扩展中UIViewController。它允许我们编写与上述相同的代码，如下所示：

```swift
func addSwiftUIView() {
    let swiftUIView = SwiftUIView()
    addSubSwiftUIView(swiftUIView, to: view)
}
```

扩展名与我们之前的代码大致相同：

```swift
extension UIViewController {

    /// Add a SwiftUI `View` as a child of the input `UIView`.
    /// - Parameters:
    ///   - swiftUIView: The SwiftUI `View` to add as a child.
    ///   - view: The `UIView` instance to which the view should be added.
    func addSubSwiftUIView<Content>(_ swiftUIView: Content, to view: UIView) where Content : View {
        let hostingController = UIHostingController(rootView: swiftUIView)

        /// Add as a child of the current view controller.
        addChild(hostingController)

        /// Add the SwiftUI view to the view controller view hierarchy.
        view.addSubview(hostingController.view)

        /// Setup the contraints to update the SwiftUI view boundaries.
        hostingController.view.translatesAutoresizingMaskIntoConstraints = false
        let constraints = [
            hostingController.view.topAnchor.constraint(equalTo: view.topAnchor),
            hostingController.view.leftAnchor.constraint(equalTo: view.leftAnchor),
            view.bottomAnchor.constraint(equalTo: hostingController.view.bottomAnchor),
            view.rightAnchor.constraint(equalTo: hostingController.view.rightAnchor)
        ]

        NSLayoutConstraint.activate(constraints)

        /// Notify the hosting controller that it has been moved to the current view controller.
        hostingController.didMove(toParent: self)
    }
}
```

这使得将视图添加到UIKit视图层次结构非常容易，甚至允许您将SwiftUI视图添加到在界面构建器中定义的容器视图。

## 结论
尽早在UIKit应用程序中添加SwiftUI视图，使您以后的生活变得更加快乐，因为您不必稍后再进行重写。使用简单的`UIViewController`扩展方法，您可以轻松地在几行代码中添加视图。确定是否可以使用SwiftUI构建新功能并利用`UIHostingController`呈现SwiftUI视图。
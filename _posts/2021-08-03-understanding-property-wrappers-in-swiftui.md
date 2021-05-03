---
layout: post
title: "理解 SwiftUI 中的属性包装器"
date: 2021-08-03 12:00:00 +0900
categories: [能工巧匠集, SwiftUI]
tags: [swift, swiftui, iOS]
---


SwiftUI 为我们提供了 ` @State`、`@Binding`、`@ObservedObject`、`@EnvironmentObject`和`@Environment` 属性包装器。因此，让我们尝试了解它们之间的区别以及我们必须使用哪个以及何时以及为什么。

#### 属性包装器
[SE-0258](https://github.com/DougGregor/swift-evolution/blob/property-wrappers/proposals/0258-property-wrappers.md)提案中描述的属性包装器功能。这里的主要目标是用逻辑包装属性，这些逻辑可以提取到分离的结构中，以便在代码库中重用它。


#### @State
`@State`是一个`Property Wrapper`，我们可以用它来描述View的状态。SwiftUI 会将其存储在`View`结构之外的特殊内部存储器中。只有相关的View才能访问它。只要`@State` 属性的值发生变化，SwiftUI 就会重建 `View`以尊重状态变化。这是一个简单的例子。

```swift
struct ProductsView: View {
    let products: [Product]

    @State private var showFavorited: Bool = false

    var body: some View {
        List {
            Button(
                action: { self.showFavorited.toggle() },
                label: { Text("Change filter") }
            )

            ForEach(products) { product in
                if !self.showFavorited || product.isFavorited {
                    Text(product.title)
                }
            }
        }
    }
}
```

在上面的示例中，我们有一个带有 `Button` 和 `List` 的简单屏幕。只要我们按下按钮，它就会更改 `state` 属性的值，并且 SwiftUI 会重新创建`View`。


#### @Binding
`@Binding`为值类型提供类似访问的引用。有时我们需要让View的状态可供其子项访问。但是我们不能简单地传递那个值，因为它是一个值类型，Swift 会传递那个值的副本。这就是我们可以使用`@Binding Property Wrapper`的地方。

```swift
struct FilterView: View {
    @Binding var showFavorited: Bool

    var body: some View {
        Toggle(isOn: $showFavorited) {
            Text("Change filter")
        }
    }
}

struct ProductsView: View {
    let products: [Product]

    @State private var showFavorited: Bool = false

    var body: some View {
        List {
            FilterView(showFavorited: $showFavorited)

            ForEach(products) { product in
                if !self.showFavorited || product.isFavorited {
                    Text(product.title)
                }
            }
        }
    }
}
```


我们使用 `@Binding` 来标记 `FilterView` 内的`showFavorited`属性。我们还使用$来传递绑定引用，因为没有$ Swift 将传递值的副本而不是传递可绑定引用。`FilterView` 可以读取和写入ProductsView的`showFavorited`属性的值。一旦`FilterView`改变了`showFavorited`属性的值，SwiftUI 就会重新创建`ProductsView`和`FilterView`作为它的孩子。

> `@Binding`为类型提供了类似访问的引用`value`。这就是为什么它应该只与值类型一起使用。如果`ValueBinding `不是值语义，`Binding`则未指定使用结果的任何视图的更新行为。


#### @ObservedObject
我们应该使用`@ObservedObject` 来处理SwiftUI之外的数据，比如你的业务逻辑。我们可以在多个独立的`Views`之间共享它，这些 `Views`可以订阅和观察该对象的更改，并且一旦更改出现，SwiftUI 就会重建绑定到该对象的所有`Views` 。让我们看一个例子。

```swift
import Combine

final class PodcastPlayer: ObservableObject {
    @Published private(set) var isPlaying: Bool = false

    func play() {
        isPlaying = true
    }

    func pause() {
        isPlaying = false
    }
}
```

在这里，我们有`PodcastPlayer`类，我们在应用程序的屏幕之间共享它。当应用程序正在播放播客剧集时，每个屏幕都必须显示浮动暂停按钮。SwiftUI 在`@Published`属性包装器的帮助下跟踪`ObservableObject`上的更改，一旦标记为`@Published`的属性发生更改，SwiftUI 就会重建绑定到该`PodcastPlayer`对象的所有视图。这里我们使用`@ObservedObject Property Wrapper`将我们的`EpisodesView`绑定到`PodcastPlayer`类


```swift
struct EpisodesView: View {
    @ObservedObject var player: PodcastPlayer
    let episodes: [Episode]

    var body: some View {
        List {
            Button(
                action: {
                    if self.player.isPlaying {
                        self.player.pause()
                    } else {
                        self.player.play()
                    }
            }, label: {
                    Text(player.isPlaying ? "Pause": "Play")
                }
            )
            ForEach(episodes) { episode in
                Text(episode.title)
            }
        }
    }
}
```

> 请记住，我们可以`ObservableObject`在多个视图之间共享，这就是为什么它必须是`reference type/class`.


#### @EnvironmentObject (环境对象)

我们可以将 `ObservableObject` 隐式注入到View层次结构的`Environment`中，而不是通过 View 的 init 方法传递`ObservableObject` 。通过这样做，我们为当前环境的所有子视图访问此`ObservableObject`创造了机会。


```swift
class SceneDelegate: UIResponder, UIWindowSceneDelegate {

    var window: UIWindow?

    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        let window = UIWindow(frame: UIScreen.main.bounds)
        let episodes = [
            Episode(id: 1, title: "First episode"),
            Episode(id: 2, title: "Second episode")
        ]

        let player = PodcastPlayer()
        window.rootViewController = UIHostingController(
            rootView: EpisodesView(episodes: episodes)
                .environmentObject(player)
        )
        self.window = window
        window.makeKeyAndVisible()
    }
}

struct EpisodesView: View {
    @EnvironmentObject var player: PodcastPlayer
    let episodes: [Episode]

    var body: some View {
        List {
            Button(
                action: {
                    if self.player.isPlaying {
                        self.player.pause()
                    } else {
                        self.player.play()
                    }
            }, label: {
                    Text(player.isPlaying ? "Pause": "Play")
                }
            )
            ForEach(episodes) { episode in
                Text(episode.title)
            }
        }
    }
}
```

如您所见，我们可以通过View的`environmentObject`修饰符传递`PodcastPlayer`对象。通过这样做，我们可以通过使用`@EnvironmentObject Property Wrapper`定义`PodcastPlayer`轻松访问它。`@EnvironmentObject`使用动态成员查找功能在 `Environment` 中查找`PodcastPlayer`类实例，这就是为什么您不需要通过 `EpisodesView` 的 init 方法传递它的原因。它像魔术一样工作。


#### @Environment (环境)

正如我们在上一章中所讨论的，我们可以将自定义对象传递到SwiftUI中`View`层次结构的`Environment`中。但是 SwiftUI 已经有一个填充了系统范围设置的环境。我们可以使用`@Environment Property Wrapper`轻松访问它们。


```swift
struct CalendarView: View {
    @Environment(\.calendar) var calendar: Calendar
    @Environment(\.locale) var locale: Locale
    @Environment(\.colorScheme) var colorScheme: ColorScheme

    var body: some View {
        return Text(locale.identifier)
    }
}
```

通过使用`@Environment Property Wrapper`标记我们的属性，我们可以访问和订阅系统范围设置的更改。一旦系统的`Locale`、`Calendar`或`ColorScheme`发生变化，SwiftUI 就会重新创建我们的`CalendarView`。


#### 结论

今天我们讲了 SwiftUI 提供的`Property Wrappers`。`@State`、`@Binding`、`@EnvironmentObject`、`@Environment` 和 `@ObservedObject`在 SwiftUI 开发中发挥着巨大的作用。
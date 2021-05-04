---
layout: post
title: "SwiftUI 中@StateObject、@EnvironmentObject 和@ObservedObject 的区别"
date: 2021-08-10 12:00:00 +0900
categories: [能工巧匠集, SwiftUI]
tags: [swift, swiftui, iOS]
---



在这篇文章中，我们将讨论 `@StateObject`、`@EnvironmentObject` 和 `@ObservedObject` 属性包装器之间的区别。我知道这是 SwiftUI 中对于新手来说最令人困惑的话题。

#### 为什么 SwiftUI 确实需要属性包装器？

SwiftUI 使用不可变的结构类型来描述视图层次结构。SwiftUI 提供的所有视图都是不可变的。这就是为什么 SwiftUI 为我们提供了一堆属性包装器来处理数据突变。属性包装器允许我们在 SwiftUI 视图中声明它们，但将数据存储在声明它的视图之外。


#### @StateObject
`@StateObject`是新的属性包装器，它初始化一个符合 `ObservableObject`协议的类的实例，并将其存储在 SwiftUI 框架的内部内存中。SwiftUI只为每个声明它的容器创建一次 `@StateObject` 并将其保持在视图生命周期之外。让我们看一些我们使用`@StateObject`来保持整个应用程序状态的示例。


```swift
import SwiftUI

@main
struct CardioBotApp: App {
    @StateObject var store = Store(
        initialState: AppState(),
        reducer: appReducer,
        environment: AppEnvironment(service: HealthService())
    )

    var body: some Scene {
        WindowGroup {
            RootView().environmentObject(store)
        }
    }
}
```

如您所见，`@StateObject`非常适合存储整个应用程序状态并与应用程序的不同场景或视图共享。SwiftUI 会将其存储在特殊的框架内存中，以将您的数据保存在场景或视图生命周期之外的安全位置。


#### @ObservedObject
`@ObservedObject`是订阅和跟踪 `ObservableObject` 更改的另一种方式。SwiftUI 不控制 `@ObservedObject` 的生命周期，你必须自己管理。`@ObservedObject`非常适合您有一个由`@StateObject`存储的`ObservableObject`并且您希望与任何可重用视图共享它的情况。



```swift
struct CalendarContainerView: View {
    @ObservedObject var store: Store<CalendarState, CalendarAction>
    let interval: DateInterval

    var body: some View {
        CalendarView(interval: interval) { date in
            DateView(date: date) {
                self.view(for: date)
            }
        }.navigationBarTitle("calendar", displayMode: .inline)
    }
}
```

我提到可重用视图是因为我在我的应用程序的多个位置使用`CalendarContainerView`，并且我不想让它依赖于环境。我使用`@ObservedObject`来明确指出该特定情况下视图使用的数据。


```swift
NavigationLink(
    destination: CalendarContainerView(
        store: transformedStore,
        interval: .twelveMonthsAgo
    )
) {
    Text("Calendar")
}
```


#### @EnvironmentObject(环境对象)
`@EnvironmentObject`是将符合 `ObservableObject` 的类的实例隐式注入视图层次结构的一部分的绝佳方式。假设您的应用中有一个包含 3-4 个屏幕的模块，并且它们都使用相同的视图模型。如果您不想将相同的视图模型从一个视图显式传递到另一个视图，那么 `@EnvironmentObject`就是您所需要的。让我们来看看如何使用它。


```swift
@main
struct CardioBotApp: App {
    @StateObject var store = Store(
        initialState: AppState(),
        reducer: appReducer,
        environment: .production
    )

    var body: some Scene {
        WindowGroup {
            TabView {
                NavigationView {
                    SummaryContainerView()
                        .navigationBarTitle("today")
                        .environmentObject(
                            store.derived(
                                deriveState: \.summary,
                                embedAction: AppAction.summary
                            )
                        )
                }

                NavigationView {
                    TrendsContainerView()
                        .navigationBarTitle("trends")
                        .environmentObject(
                            store.derived(
                                deriveState: \.trends,
                                embedAction: AppAction.trends
                            )
                        )
                }
            }
        }
    }
}
```


在上面的示例中，我们将环境对象注入到`SummaryContainerView`的视图层次结构中。SwiftUI 将隐式地为所有位于`SummaryContainerView`中的子视图提供对插入的环境对象的访问权限。我们可以使用`@EnvironmentObject`属性包装器快速获取和订阅注入的环境对象。


```swift
struct SummaryContainerView: View {
    @EnvironmentObject var store: Store<SummaryState, SummaryAction>

    var body: some View {
        //......
    }
}
```

不得不提的是 `@EnvironmentObject`与`@ObservedObject`具有相同的生命周期。这意味着无论何时在可以由 SwiftUI 重新创建的视图中创建它，都可以获得一个新的环境对象。

#### 结论
今天我们讨论了`@StateObject`、`@EnvironmentObject` 和 `@ObservedObject` 属性包装器之间的区别。

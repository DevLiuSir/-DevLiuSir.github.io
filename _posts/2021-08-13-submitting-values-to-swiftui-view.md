---
layout: post
title: "SwiftUI 视图提交值"
date: 2021-08-13 12:00:00 +0900
categories: [能工巧匠集, SwiftUI]
tags: [swift, swiftui, iOS]
---



`SwiftUI 3.0` 为我们带来了一种新的声明式方法来处理提交的值。文本字段、表单、搜索栏允许用户提交我们可以使用的值并使用新的 `onSubmit`视图修饰符对它们做出反应。本周我们将学习如何使用`onSubmit`视图修饰符以及它为我们带来的好处。

#### 基本
让我们构建一个允许我们使用`searchable`可搜索视图修饰符 搜索消息的视图。


```swift
struct SearchView: View {
    @ObservedObject var viewModel: ViewModel
    @Binding var query: String

    var body: some View {
        List(viewModel.messages, id: \.self) { message in
            Text(message)
        }
        .searchable(text: $query)
        .onSubmit(of: .search) {
            viewModel.search(matching: query)
        }
        .navigationTitle("Search")
    }
}
```

正如您在上面的示例中所见，我们显示来自`ViewModel的`消息列表并提供搜索功能。我们还使用`onSubmit`视图修饰符来提供一个闭包，只要用户提交值，SwiftUI 就会运行该闭包。一旦用户点击软件或硬件键盘上的返回键，SwiftUI 就会调用提供的闭包。


我们将`onSubmit`视图修饰符与搜索提交触发器一起使用。这意味着 SwiftUI 仅作为搜索操作的结果运行给定的闭包。SwiftUI 为我们提供了一组不同的提交触发器，如搜索、文本、表单，并且在 SwiftUI 的未来版本中其数量会增加。

我们可以与`onSubmit`视图修饰符一起使用的其他视图是`TextField`和`SecureField`。我们可以将`onSubmit`视图修饰符直接附加到文本字段。在这种情况下，我们必须使用文本提交触发器。

```swift
struct ContentView: View {
    @State private var query = ""

    var body: some View {
        NavigationView {
            TextField("query", text: $query)
                .onSubmit(of: .text) {
                    print(query)
                }
        }
    }
}
```

请记住，我们可以将具有各种提交触发器的多个`onSubmit`视图修饰符附加到视图层次结构，并为单独的触发器提供不同的闭包。

我们还可以使用`submitLabel`视图修饰符更改软件键盘上返回键的标签。`submitLabel`视图修饰符需要`SubmitLabel`结构的实例 作为定义返回键标签的参数。它有许多预定义的值，例如`done`、`go`、`send`、`join`、`route`、`search`、`return`、`next` 和 `continue`。


```swift
struct ContentView: View {
    @State private var query = ""

    var body: some View {
        NavigationView {
            TextField("query", text: $query)
                .submitLabel(.send)
                .onSubmit(of: .text) {
                    print(query)
                }
        }
    }
}
```

#### 范围

我应该提一下，您不仅可以将`onSubmit`视图修饰符放置在文本字段下，还可以放置在视图层次结构中的任何位置。这就是为什么 SwiftUI 为我们提供了控制提交范围的机会。例如，您可以禁用视图层次结构的一部分以对提交值做出反应。


```swift
struct ContentView: View {
    @StateObject private var viewModel = ViewModel()

    var body: some View {
        NavigationView {
            VStack {
                TextField("phone", text: $viewModel.phone)
                    .submitScope(viewModel.phone.count > 11)

                VStack {
                    TextField("email", text: $viewModel.email)
                    TextField("password", text: $viewModel.password)
                }
            }
            .onSubmit(of: .text) {
                viewModel.signUp()
            }
        }
    }
}
```

`submitScope`视图修饰符允许您避免特定视图调用提交操作。在我们的示例中，当提供的条件为 false 时，电话文本字段不会启动提交。

#### 结论
本周，我们了解了新的`onSubmit`视图修饰符，它为我们提供了一种通用方式来处理不同视图的值提交。



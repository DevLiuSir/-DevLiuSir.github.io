---
layout: post
title: SwiftUI 点击List的Row时Push到一个新的View
date: 2020-06-25 12:00:00 +0900
categories: [能工巧匠集, SwiftUI]
tags: [swift, swiftui, iOS, List, NavigationLink]
---

通常我们在列表 **UITableView** 或者 **UICollectionView** 中点击 Cell 来 Push 到一个新的页面会使用对应的代理方法 `didSelectRowAt`，然而在 SwiftUI 中没有对应的代理属性，也没有类似的可以 push 的API，那么如何在 SwiftUI 中 push 一个 View 呢？那就是使用 **NavigationLink**。

在 SwiftUI 中 **NavigationLink** 首先是一个 View，而且他可以控制导航 `presentation`，因此我们可以利用这个特性来实现当列表的某个Cell 被点击的时候可以 push 到一个新的页面。通常我们实现信息列表如下所示：

```swift
struct Message: Identifiable {
    var id = UUID()
    var content: String
    var number: String
}

struct MessageRow: View {
    var message: Message
    var body: some View {
        VStack(alignment: .leading) {
            Text(message.content)
                .font(.body)
            Text(message.number)
                .font(.callout)
        }
    }
}

struct MessageListView: View {
    private var messages = [
        Message(content: "您尾号8888卡，余额1000,000,000。【工商银行】", number: "95588"),
        Message(content: "感谢您申请招行信用卡，下载掌上生活，自助开卡。【招商银行】", number: "95555"),
        Message(content: "当前话费余额：1000.2元。", number: "10086"),
        Message(content: "Apple Store：订单*****中的商品已配送成功。希望您喜欢。", number: "10069030049001"),
    ]
    
    var body: some View {
        NavigationView {
            List(messages) { message in
                MessageRow(message: message)
            }
                .navigationTitle("信息")
        }
    }
}
```

下面的 MessageDetail 是 Cell 点击后展示详细的信息视图，

```swift
struct MessageDetail: View {
    var message: Message
    var body: some View {
        VStack {
            Text(message.content)
        }
            .navigationTitle("来自\(message.number)的消息")
    }
}
```

现在一切准备就绪，只需要实现点击跳转的功能了。那么我们需要做的就是把列表的 Cell 用 `NavigationLink` 包起来就能实现push的功能了。封装后 Cell 的代码如下


```swift
NavigationLink(
    destination: MessageDetail(message: message),
    label: {
        MessageRow(message: message)
    })
```
    

其实就是用 **NavigationLink** 把 **MessageDetail(message:)** 和 **MessageRow(message:)** 链接了起来，`NavigationLink` 收到点击的事件后执行了 push 操作。下面是完整的代码：


```swift
struct Message: Identifiable {
    var id = UUID()
    var content: String
    var number: String
}

struct MessageRow: View {
    var message: Message
    var body: some View {
        VStack(alignment: .leading) {
            Text(message.content)
                .font(.body)
            Text(message.number)
                .font(.callout)
        }
    }
}

struct MessageListView: View {
    private var messages = [
        Message(content: "您尾号8888卡，余额1000,000,000。【工商银行】", number: "95588"),
        Message(content: "感谢您申请招行信用卡，下载掌上生活，自助开卡。【招商银行】", number: "95555"),
        Message(content: "当前话费余额：1000.2元。", number: "10086"),
        Message(content: "Apple Store：订单*****中的商品已配送成功。希望您喜欢。", number: "10069030049001"),
    ]
    
    var body: some View {
        NavigationView {
            List(messages) { message in
                NavigationLink(
                    destination: MessageDetail(message: message),
                    label: {
                        MessageRow(message: message)
                    })
            }
            .navigationTitle("信息")
        }
    }
}


struct MessageDetail: View {
    var message: Message
    var body: some View {
        VStack {
            Text(message.content)
                .font(.headline)
                .padding(EdgeInsets(top: 0, leading: 20, bottom: 0, trailing: 20))
                .lineLimit(2)
                //设置多行文本的对齐方式
                .multilineTextAlignment(.leading)
        }
        .navigationTitle("来自\(message.number)的消息")
    }
}

```

### 效果

![](/assets/images/2020/NavigationLink/message.gif)
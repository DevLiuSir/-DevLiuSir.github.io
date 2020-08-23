---
layout: post
title:  macOS 开发之 Windows 和 WindowController
date:   2019-12-18 22:21:49
categories: [能工巧匠集, macOS]
tags: [swift, macOS开发]
---

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjypwzgsmgj31kw0no0tg.jpg)

本文带大家一起简单学习如何开发一个基于文档的 `Cocoa` 应用(`document based Cocoa App`)，并学习如何使用模态窗口(`Modal Windows`) 以及 `macOS Sierra` 支持的标签窗口(`tabbed interface`)。

### 开发平台

> - macOS 10.14.6
- Swift 5.1
- xcode 11.3


### 概述

所有 macOS 程序要呈现用户界面都是以 Windows 作为容器的，当然除了纯粹的命令行工具和菜单栏小工具。Windows 定义了 App 在屏幕中的活动区域，在这个区域内允许用户进行易于理解的多任务交互操作。macOS 应用最终可分为以下几种：

- 单窗口的工具，比如计算器

![](https://tva1.sinaimg.cn/large/00831rSTly1gdlac6zjdej30u01cv0ur.jpg)

- 单窗口的 `library-style`应用，比如照片应用(Photos.app)
![](https://tva1.sinaimg.cn/large/00831rSTly1gdlac7bte0j31cv0u0767.jpg)


- 基于文档的多窗口应用，比如文本编辑应用(TextEdit.app)

![](https://tva1.sinaimg.cn/large/00831rSTly1gdlac76ty7j31cv0u0q5h.jpg)

不管属于哪一类的应用，几乎每一个 macOS 应用都是使用 MVC (Model-View-Controller) 构建的，这是 macOS 开发中核心的开发模式。

Cocoa 应用中，一个窗口是 `NSWindow` 类的一个实例(window 就是 view 的容器)，其与控制器紧密配合工作，而控制器是 `NSWIndowController` 的一个实例。在一个设计良好的应用中会发现通常窗口和控制器是一一对应的，而 MVC 模式中的第三个组成部分——模型(model)的使用会根据应用的类型和设计而不同。

在本文中我们会创建一个有点像 `TextEdit` 的应用，是 `document based` 的，我管它叫 TextEditor，这个名字随便，只要您喜欢啥都行！在我们开发过程中，将会涉及到以下内容：

- 窗口 和 窗口控制器
- document 架构
- NSTextView
- 模态窗口
- 菜单栏和菜单项


### 搞起

启动 Xcode，按快捷键 `Shift`+ `Command` + `n` ，新建工程，选择 macOS -> Cocoa App，点击 Next：

![](https://tva1.sinaimg.cn/large/00831rSTly1gdlac7lsfrj316u0u00w3.jpg)

新出现的窗口中，勾选 `Create Document-Based Application`，应用的名称随意取，比如 `TextEditor`，然后 `Document Extention` 这一项指定文件的扩展名，其决定以后应用保存文件的扩展名，比如这里指定的是 LC，可以不勾选包含测试，点击 Next：

![](https://tva1.sinaimg.cn/large/00831rSTly1gdlarrhccbj312m0u012y.jpg)

选择合适的目录，点击 `Create` 创建即可，创建成功后编译运行，你会看到类似下面的窗口：



此时，按快捷键 Command + n 或者菜单栏点击 File -> New，可以创建新的窗口如下：

![](https://tva1.sinaimg.cn/large/00831rSTly1gdlb21frr4j31aj0u01ky.jpg)


### Document-Based 应用如何工作

上面也看到了这类应用的样子，下面花几分钟一起看一下 Document-Based 应用如何工作的。

#### Document 的结构

一个 Document 其实就是 `NSDocument` 的一个实例对象，文档模型 `NSDocument` 作为模型保存了文档的数据，同时负责文档窗口控制器 `NSWindowController` 的创建管理，它管理文档数据，负责文档打开时数据读取的管理和文档对象管理的数据保存到文件的处理，而文件有可能是在硬盘也有可能在 iCloud，均支持。

创建关联的 `NSWindowController` 负责展示文档的内容，内容视图最终相应处理用户对文档操作的各种交互事件。

`NSDocumentController` 是一个单例对象，主要负责文档模型 `NSDocument` 的管理工作，维护系统中所有的文档模型 `NSDocument` 列表，控制多个文档窗口的激活切换，同时跟踪当前活动文档对象，最终结构图如下：

![](https://tva1.sinaimg.cn/large/00831rSTly1gdlb50jls2j31kw0u0q3a.jpg)


### 禁用 Document 的保存和打开

Document-Based 应用的 Documnent 架构支持文件的保存和打开，但是需要定义 Document 类中自己具体实现，本文暂不涉及这部分内容，所以先禁用 Document 的保存和打开。

打开文件 Document.swift，会发现有两个函数是空的：

- `data(ofType:)`: 用于写文件
- `read(from:ofType:)`: 用于读取文件

同时还有一个函数是 `autosavesInPlace()->Bool`，更改返回值为 `false`:


```swift
override class var autosavesInPlace: Bool {
    return false
}
```

这样我们首先禁用了自动保存功能，下面我们需要禁用菜单栏 File 中的保存和打开菜单项。在这之前，我们运行程序，点击 File -> Open ，竟然会弹出打开文件的窗口，很神奇，我们并没有实现打开呀，为什么会出现文件打开窗口呢？

![](https://tva1.sinaimg.cn/large/00831rSTly1gdlbb5oid9j31ct0u0goh.jpg)

其实是因为 Open 的菜单项绑定了具体的 Action，Action中实现了这些，所以我们只需要断开菜单项与 Action 的链接就可以禁用掉菜单项，视觉上的表现就是菜单栏相应菜单项会变灰色不可用。

打开 `Main.storyboard` ，找到 `Application Scene`，点击菜单栏中 `File`，选择其中的 Open，右击会看到所有的连接，点击 Sent Action 中连接右侧的 x 号，即可断开连接：

![](https://tva1.sinaimg.cn/large/00831rSTly1gdlbdk3d2bj31860t8wil.jpg)

同样的操作，分别将 `Save...`、`Save As...` 和 `Reverrt to Saved` 与相应的 Action 断开连接。

然后删除 `Open Recent` 菜单项，最后我们重写一下 `save(withDelegate:didSave:contexInfo:)` 方法，后面我们会用到，主要是添加一个错误提醒窗口，打开 `Document.swift` 文件，在 `Document` 类中添加重写方法如下：


```swift
override func save(withDelegate delegate: Any?, didSave didSaveSelector: Selector?, contextInfo: UnsafeMutableRawPointer?) {
    let userInfo = [NSLocalizedDescriptionKey: "Sorry, no saving implemented in this post. Click 'Do not save' to quit!"]
    let error = NSError(domain: NSOSStatusErrorDomain, code: unimpErr, userInfo: userInfo)
    let alert = NSAlert(error: error)
    alert.runModal()
}
```


运行程序，此时会看到菜单栏的相应菜单项已经禁用：

![](https://tva1.sinaimg.cn/large/00831rSTly1gdlblon4wlj30cc0csmxa.jpg)



### 窗口显示

上面新建文件的时候，我们发现新建文档的窗口完全覆盖了之前的文档窗口，然而这不是我们想要的结果，本节就聊一下怎么合理布局窗口的位置。进行改造前，我们需要新建一个 `NSWindowController` 的子类，然后添加相应的代码实现我们预期的功能。


#### 新建 NSWindowController 的子类

工程导航栏选择 `TextEditor`，按下快捷键 `Command + n`，就会弹出新建文件的导航窗口，选择 `macOS` -> `Source` -> `Cocoa Class`，点击 `Next`

![](https://tva1.sinaimg.cn/large/00831rSTly1gdlboblxfsj318k0u07oq.jpg)

取名为 `WindowController`，选择继承自 `NSWindowController`，不要勾选 `Also Create XIB file for user interface`，语言选择 `Swift` ，点击 `Next`，之后默认然后点击 `Create` 即可创建。

下一步需要确保 `storyboard` 中的 `window` 的控制器是我们刚定义的 `WindowController` 的实例，打开 `Main.storyboard` 点击 `Window Controller Scene` 中的 `Window Controller`，按快捷键 `Option + Command + 3` ，在右侧显示的 `Identity Inspector` 中配置 `Custom Class`为 `WindowController`，也就是刚刚创建的类：

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjypet2tomj31400osaco.jpg)


### 层叠窗口

现在我们可以使新建的窗口层叠显示而不是覆盖显示，打开新建的 `WindowController.swift` 文件，添加以下代码：

```swift
required init?(coder: NSCoder) {
    super.init(coder: coder)
    shouldCascadeWindows = true
}
```

只需要设置 `shouldCascadeWindows` 为 `true` 就可以实现层叠效果，运行程序测试一下：

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjypeszgcoj30h80b4jri.jpg)

### 以标签页显示

层叠效果很不错，但我们可以尝试一下其它的方式，比如从 `macOS Sierra` 开始新增加的 `tabbed Windows`，简单说就是新建的窗口以标签页显示。

打开 `Main.storyboard`，选中 `Window Controller scene` 下的 `Window` ，然后打开 Inspector 栏的 `Attributes Inspector` (可以按快捷键 `Option + Command + 4`)，找到 `Tabbing Mode`，更改它的值为 `Preferred`:

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjypesv36bj31400ostbb.jpg)

运行程序，然后按快捷键 `Command + n` 新建窗口，可以看到新的窗口以标签页的方式显示了：

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjypesr861j30gg0b8aa4.jpg)

当我们运行程序的时候，macOS 会根据当前屏幕大小和应用请求窗口的大来决定应用窗口的显示位置和实际的大小，下面我们将学习两种方式控制应用窗口的显示位置和实际大小。

### 使用 `Interface Builder` 设置窗口显示位置

首先我们需要先使用 `Interface Builder` 设置窗口的初始位置。

打开 `Main.storyboard`，选中 `Window Controller scene` 下的 `Window` ，然后打开 `Inspector` 栏的 `Size Inspector` (可以按快捷键 `Option + Command + 5`)，找到 `Initial Position`，运行程序后的窗口就是按照这个设置初始位置的：

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjypesnullj31400miq4v.jpg)

其中 x 代表窗口到屏幕左边缘的距离，y 代表窗口到屏幕底边的距离，单位是 `px`，在 `macOS` 中应用的坐标原点在左下角，这与 `iOS` 中是不同的（`iOS` 使用的是 `flipped` 坐标系，其原点在左上角）。


![](https://tva1.sinaimg.cn/large/0081Kckwly1gjypesgkkuj31400qc0tp.jpg)


我们可以点击上面 `Size Inspector` 中的窗口位置预览图中的红色 `constrains`，这会决定 macOS 显示应用窗口位置的设定，点击红色 `Constrains` 可以打开或关闭相关限制，同时会看到下面两个下拉框得值会改变，比如这里：

- 取消上边和右边的红色限制，此时会看到下面两个下拉框的值分别变成 `Fixed From Left` 和 `Fixed From Bottom`
- 设置初始的位置：x -> 200, y -> 200

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjypesbpzlj30dq0bu3ys.jpg)

此时重新编译运行应用，你会发现不管屏幕多大，只要尺寸允许范围内，应用的窗口会显示在离屏幕左边和底边均为 200 px 的位置。

⚠️: `macOS` 会记住 `app` 的窗口显示位置，所以需要先把应用完全退出，然后再编译运行就能看到修改的效果！

### 代码实现对窗口显示位置的设置

代码实现的话，需要在 `window` 加载之后进行设置，在 `WindowsController` 中重写的 `windowDidLoad` 方法中添加相关代码。

这次我们来点特别的，我们设置窗口显示在离屏幕顶边和左边均为 150px 的位置，打开 `WindowController.swift` 文件，在 `WindowController` 类中修改 `windowDidLoad` 方法内容如下：

```swift
override func windowDidLoad() {
    super.windowDidLoad()
    
    if let window = window, let screen = window.screen {
        let offsetFromLeftOfScreen = CGFloat(150)
        let offsetFromTopOFScreen = CGFloat(150)
        let screenFrame = screen.visibleFrame
        let offsetFromBottomOfScreen = screenFrame.maxY - window.frame.height - offsetFromTopOFScreen
        window.setFrameOrigin(CGPoint(x: offsetFromLeftOfScreen, y: offsetFromBottomOfScreen))
    }
}
```

上面代码主要完成以下工作：

- 获取需要用到的 `NSWindow` 和 `NSScreen` 的实例
- 得到 `screen` 的 `visibleFrame`
- 通过离顶边的距离计算得到离底边距离
- 设置 `window` 的远点坐标为 (`offsetFromLeftOfScreen, offsetFromBottomOfScreen`)

选中之前显示的应用窗口，`Command + q` 完全退出应用，然后回到 `Xcode` 编译运行应用，会看到应用的窗口如期显示在指定的位置：

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjypnj8xqbj30iw0egwen.jpg)

### 变身超 mini 富文本处理工具

Cocoa 有很多可以添加到 window 中的牛🐂的功能性 UI 控件，在本节我们将会用到 `NSTextView`，在这之前，我们需要了解 `NSWindow` 的 `content view`。

### content view

`contentView` 位于 `window` 中视图层次的根级，在这个视图中我们可以放置所有界面元素。另外，我们还能替换默认的 `contentView` 为我们自定义的视图，在这里我们就不做相关操作了，以后我们可能会用到！

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjypnj4e99j31400txtaw.jpg)

### 添加 Text View

打开 `Main.storyboard` 文件，找到 `View Contorller Scene` 下的 `View Controller`，其下的 `View`中有个控件 `Your document contents here` ，将其删除，然后我们添加 `Text View`:

- 按快捷键 `Shift + Command + l` 打开 `Object Library`

- 搜索 Text View

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjypnj1enrj30dw0dydgm.jpg)

- 将 `Rich Document Content Text View` 拖入 `Content View` 中

- 调整 `Rich Document Content Text View` 的大小和位置，最终使其四边分别与 `contentView `的边缘贴齐

- 选中刚添加的 `Text View` 控件，然后点击底边的 `Resolve Auto Layout Issues` 按钮，选中 `Reset To Suggested Constrains`

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjypniy7d1j31400s6juf.jpg)

- 添加限制之后的样子如下：

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjypniupshj31400s6772.jpg)


编译运行，可以看到刚添加的 Text View 了：

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjypniqkb8j30gg0b8q2z.jpg)

在窗口的 Text View 中可以添加进行文本编辑了，也支持常用的快捷键，比如复制、粘贴、剪切、撤销、重做等。窗口中也出现了一组工具栏，支持字体设置、简单的段落设置等，同时菜单栏的 Format 的菜单项功能也是可用的，还支持查找替换。这一小节我们没有添加任何代码，就完成了一个简单的富文本编辑工具了，是不是炒🐔煎🍳！

### 撤销和重做

在窗口中添加部分文本，已经可以完成基本的富文本编辑功能了，但是此时还不支持撤销和重做，我们需要添加支持。

- 打开 `Main.storyboard` 文件，依次找到 `View Controller -> View -> Scroll View - Text View -> Clip View -> Text View`，选中 `Text View`
- 按下快捷键 Option + Command + 4 打开 `Attribute Inspector`，勾选 `Undo` 复选框

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjyq808mnxj31400s60vw.jpg)

此时运行程序，就支持 Undo 和 Redo 了！

在文本框添加文本以后，我们点击窗口关闭按钮，此时会提醒要不要保存文档：

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjypnintuyj30gg0b80t0.jpg)

点击 `save` 按钮，会弹出一个警告窗口：

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjypnikpumj30es07bwfh.jpg)


是不是对里面的内容很熟悉，这就是前面添加的 save 方法中的错误信息。

### 模态窗口(Modal Window)

模态窗口是一种特殊的窗口，一旦显示就会独占用户的所有操作事件，一直到它被关闭，其它窗口才能响应用户的操作。

显示模态窗口有三种方法：

- 以一个普通窗口的形式显示，使用 `NSApplication.runModal(for:)` 触发显示
- 以 `Modal sheet` 的形式显示， 调用 `NSWindow.beginSheet(_:completionHandler:)` 显示窗口
- 通过模态会话的形式，本文暂不涉及这种高级的方法


其实，文档的保存和打开窗口就是模态窗帘的好例子，就像上面关闭窗口时弹出的提示保存的窗口，它出现在窗口的顶部，这就是 Modal Sheet，在本文也不讲这种模态窗口，下面我们一起实现一个显示字数和段落统计的模态窗口，它是以一个正常窗口形式显示的。

### 添加一个新的窗口

打开 `Main.storyboard` 文件，按快捷键 `Shift + Command + l` 打开 `Object Library`，搜索 Window Controller，拖拽 `Window Controller` 进入画布，这会生成两个场景：`Window Controller Scene` 和 `View Controller Scene`。

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjyqc5ygxbj31400mijt2.jpg)

选中刚添加的 `WIndow Controller Scene` 中的 Window，按快捷键 Option + Command + 5，打开 `Size Inspector`，调整其宽为 300，高为 150。

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjyolh4g4kj30wc0msq5f.jpg)

继续选中 Window，按快捷键 Option + Command + 4 打开 Atrribute Inspector，取消 Close、Resize 和 Minimise 控件复选框的勾选，设置标题为 Word Count。

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjyolh0puwj30wc0msgo6.jpg)

窗口 Close 按钮会造成一个 bug：当点击这个按钮后虽然窗口已经关闭，但是应用因为没有调用 stopModal 方法而一直保留在模态状态，这就很尴尬了！

另外，不保留 Minimise 和 Resize 按钮是为了遵循 Apple 的 [Human Interface Guidelines (HIG)](https://link.zhihu.com/?target=https%3A//developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)。

选中新添加的 View Controller Scene 中的 View，按下快捷键 Option + Command + 5 打开 Size Inspector，设置宽为 300 高为 150。

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjyolgwwiaj30wc0mswgi.jpg)

### 配置 Word Count 窗口

Shift + Command + l 打开 Object Library 拖拽 4 个 label 到 View 中。


![](https://tva1.sinaimg.cn/large/0081Kckwly1gjyolgsg4aj30wc0ms0v5.jpg)

改变四个 label 的标题分别为：Word Count、Paragraph Count、0 和 0，同时设置它们都是右对齐，调整它们的宽为 120，这里我们不涉及自动布局，可能会出现几个警告，先不管它们。

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjyolgo8qqj30wc0msjua.jpg)

从 Object Library 推拽一个 Push Button 到 View 中，更改其标题为 OK，手动调整所有控件布局到合适的位置。


![](https://tva1.sinaimg.cn/large/0081Kckwly1gjyolgke47j30wc0mswh7.jpg)


### 创建 Word Count 的 View Controller 的类

Command + n 会打开一个文件新建的导航窗口，我们选择 macOS -> Source -> Cocoa Class，新出现的窗口中输入类的名称为 `WordCountViewController`，Subclass of 设置为 `NSViewController`，取消勾选 `Also create XIB for user interface`

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjyolggue7j30wc0msjt6.jpg)

点击 Next 创建新的文件。

打开 `Main.storyboard`，选中新添加的 `View Controller`，按快捷键 `Option + Command + 3` 打开 `Identity Inspector`，选择 `class` 为刚添加的 `WordCountViewController` 类。


![](https://tva1.sinaimg.cn/large/0081Kckwly1gjyolggue7j30wc0msjt6.jpg)


### 绑定计数 label 与 View Controller

打开新建的 WordCountViewController.swift 文件，在 WordCountViewCOntroller 中添加属性如下：

```swift
@objc dynamic var wordCount: Int = 0
@objc dynamic var paragraphCount: Int = 0
```

⚠️：两个属性添加了 @objc dynamic 修饰符是为了有效实现 Cocoa Bindings^[Cocoa Bindings 是 UI 开发中一个强大的技术，主要用于数据与 UI 的绑定，可以阅读 [Cocoa Bindings on macOS](https://link.zhihu.com/?target=https%3A//www.raywenderlich.com/141297/cocoa-bindings-macos) 了解更多相关内容，后面有时间十里会专门写一篇相关的文章与大家一起学习！]，否则绑定无效运行时会报错。

打开 `Main.storyboard` 选中与 `Word Count` 的 label 相对应的数字 label，按下快捷键 Option + Command + 7 打开 `Bindings Inspector`:

- 点击 Value 左边的小三角，展开 Value
- Bind to 的下拉框选择 `Word Count View Controller`
- 勾选 Bind to
- `Model Key Path` 输入 `wordCount`


![](https://tva1.sinaimg.cn/large/0081Kckwly1gjyolg4ngcj30wc0ms770.jpg)

同样的步骤，与 `Paragraph Count` 的 label 相对应的数字 label 绑定到 `paragraphCount`:

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjyolfzrzkj30wc0mswh9.jpg)

下一步设置 `Window Controller` 的 `Storyboard ID`。

选择 `Word Count Window` 的 `Window Controller`，然后按快捷键 Option + Command + 3 打开 Identity Inspector，更改 Storyboard ID 的值为 `Word Count Window Controller`。


![](https://tva1.sinaimg.cn/large/0081Kckwly1gjyolfufq1j30wc0ms0v4.jpg)


### 显示 和 关闭模态窗口

前面的准备工作做足了，那本节讲讲如何召唤和轰走模态窗口。

### 出来吧，模态窗口

打开 `ViewController.swift` 文件，在类中添加以下属性：

```swift
@IBOutlet var text: NSTextView!
```

同时添加以下方法：

```swift
@IBAction func showWordCountWindow(_ sender: AnyObject) {
    // 1
    let storyboard = NSStoryboard(name: "Main", bundle: nil)
    let wordCountWindowController = storyboard.instantiateController(withIdentifier: "Word Count Window Controller") as! NSWindowController

    if let wordCountWindow = wordCountWindowController.window, let textStorage = text.textStorage {
    
        // 2
        let wordCountViewController = wordCountWindow.contentViewController as! WordCountViewController
        wordCountViewController.wordCount = textStorage.words.count
        wordCountViewController.paragraphCount = textStorage.paragraphs.count
    
        // 3
        NSApplication.shared.runModal(for: wordCountWindow)
        // 4
        wordCountWindow.close()
    }
}
```


打开 `Main.storyboard` 文件，选中添加 Text View 的 View Controller，按住 Ctrl 键，点击 `View Controller` 按钮，不松手拖动至 Text View 上松手，此时会弹出一个绑定选择框，里面就包含了我们刚添加的 text 属性，点击它，这就完成了 Text View 控件与 text 属性的绑定

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjyolfql4rj30wc0msgnt.jpg)

上面添加的方法，这里一步一步的说明一下：

使用之前配置的 `Storyboard ID` 实例化一个 `Word Count Window Controller` 对象
从 `text view` 的 `storage` 对象中获取字数统计和段落统计，将值设置到 `wordCountViewController` 的两个属性 `wordCount` 和 `paragraphCount`
模态方式显示 `word count` 窗口
一旦模态状态结束就关闭模态窗口，这里需要注意，只要模态不结束这一句就不会执行

### 消失吧，模态窗口

这里我们需要添加结束模态的实现，打开文件 `WordCountViewController.swift`，添加以下方法：


```swift
@IBAction func dismissWordCountWindow(_ sender: NSButton) {
    NSApplication.shared.stopModal()
}
```

下面我们将此方法与上面添加的 OK 按钮进行绑定。

打开 `Main.storyboard`，选中 `OK` 按钮，点击它，同时按住 `Ctrl` 键，拖动鼠标至 `Word Count View Controller` 的按钮上，在弹出的绑定窗口上选择刚添加的方法 `dismissWordCountWindow` 即可完成绑定。

![](https://tva1.sinaimg.cn/large/0081Kckwly1gjyolfn8mcj30wc0mstb8.jpg)

### 添加召唤模态的符咒

这里我们以菜单栏的菜单项的方式触发模态窗口。

打开 `Main.storyboard` 文件，找到 Main Menu，点击展开 Edit，然后进行以下操作：

Shift + Command + l 打开 Object Library，搜索 Menu Item，拖动到最下面的位置，添加一个新的菜单项，选中它
Option + Command + 4 打开它的 `Attribute Inspector`，更改标题为 `Word Count`，同时配置快捷键为 ⌘K


![](https://tva1.sinaimg.cn/large/0081Kckwly1gjyolfg27kj30wc0msn05.jpg)

下面我们需要为其绑定上面定义的方法 `showWordCountWindow`，点击菜单项 `Word Count`，同时按住 Ctrl 键，拖动至 `Application Scene` 下的 `First Responder` 上松手，在弹出的列表中找到方法 `showWordCountWindow`，选择它，这就完成了触发模态的绑定：


![](https://tva1.sinaimg.cn/large/0081Kckwly1gjyoyg6yj9j30wc0msq66.jpg)

### 召唤模态

编译运行程序，在窗口中输入一些内容，比如：

>望岳
>唐代：杜甫

>岱宗夫如何？齐鲁青未了。

>造化钟神秀，阴阳割昏晓。

>荡胸生曾云，决眦入归鸟。

>会当凌绝顶，一览众山小。

菜单栏 `Edit` -> `Word Count`(或者按快捷键 `Command + k`) 就能打开统计字数的模态窗口。
![](https://tva1.sinaimg.cn/large/0081Kckwly1gjyolfa4t8j30gg0b8mxl.jpg)


### 点击 OK 就可以“轰走”模态窗口了。

总结

- MVC 的设计模式
- 多窗口应用的实现
- Interface Builder 和 代码 两种方式控制窗口位置
- 控件与类属性的绑定，控件 Action 与类方法的绑定
- 窗口形式的 macOS 的常规开发姿势
- 如何代码控制显示模态窗口
- 富文本编辑的简单实现

希望对大家学习 macOS 开发有所帮助！感谢您的阅读！

参考


[Windows and WindowController Tutorial for macOS](https://www.raywenderlich.com/613-windows-and-windowcontroller-tutorial-for-macos)
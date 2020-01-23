---
layout: post
title:  macOS开发系列教程的第二部分！
date:   2017-11-12 22:26:49
categories: macOS
tags: macOS
---

[原文](https://link.jianshu.com/?t=https%3A%2F%2Fwww.raywenderlich.com%2F151746%2Fmacos-development-beginners-part-2)

在第一部分，你学了怎样安装 Xcode，怎样创建一个新的 APP，添加 UI，连接 UI 和代码，调试代码和怎样获得帮助。如果你对这些还不太确定，回到第一部分再浏览一遍。
在这一部分，你将创建一个更复杂的 APP 的用户接口。你将学到怎样让一个窗口可调整大小，已经设计和导航到第二个窗口显示 APP 的偏好设置。


### 准备开始

打开 Xcode， 在欢迎窗口中点击 `Create a new Xcode project`，或者选择 `File/New/Project…`就像在第一部分一样。选择 `macOS/Application/Cocoa Application`，点击下一步，将 APP 命名为 `EggTimer`，确保语言选择 `Swift`， `Use Storyboards` 是选中的。点击 Next 选择项目保存的地方。

编译运行 APP，确保它工作正常。

#### EggTimer App

你即将创建的 `APP` 是 `EggTimer App`。它从选择的时间开始倒数计数剩余的时间。有一幅图片鸡蛋熟的时候会改变，鸡蛋准备好的时候会播放音乐。第二个窗口将显示 APP 的偏好。

![image.png](https://upload-images.jianshu.io/upload_images/323996-cdc1515617f79599.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)


从项目导航器打开 `Main.storyboard`。像在第一部分中看到的一样，你已经拥有了 3 个组件：

- Application Scene
- Window Controller Scene
- View Controller Scene


####  Application Scene

包含菜单栏和菜单，程序运行时它会出现。`Window Controller `定义了程序窗口的行为：它怎样调整大小，新窗口怎么显示，APP 是否保存窗口大小和位置等等。一个窗口控制器可以管理多个窗口，但是如果它们需要不同的属性，你需要添加多个 `Window Controller`。

`View Controller` 显示窗口中的用户接口 --> 这是你布置 UI 的地方。

注意，有一个箭头指向 `Window Controller`。这表明当 APP 启动时，它将控制最初的显示。你可以通过选择文档大纲中的 `Window Controller`，然后查看 `Attributes Inspector` 来确认这点。取消 `Is Initial Controller`选项箭头就会消失。再次选中它，因为你确实想让它成为最初的控制器。

#### Window Controller

开始用户接口之前确保选择了 `Main.storyboard`。在 `Window Controller` 里点击选择窗口。在 `Window Controller` 可视化编辑器了显示了` “View Controller”`，因为它是包含在控制器里的。对于这个 APP，你不想让它缩小到 346 x 471以下。这也是这个窗口的初始大小。

![image.png](https://upload-images.jianshu.io/upload_images/323996-414cbb94676d0ace.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

到 `Size Inspector` 里将内容宽度设置为 346，高度设置为 471。检查 `Minimum Content Size`，确保最小宽度和高度与内容大小一致。可视编辑器中的 `Window Controller` 大小将改变。你可能会想移动一下它，不让它覆盖其他对象。

虽然没有严格的必要，如果你把 `View Controller` 的尺寸调整为跟包含它的 Window Controller 一样，可视化界面看起来更容易。点击 `View Controller`，确保在文档大纲里视图是选中的。在尺寸检查器了将宽和高分别设置为 346 和 471。如果需要，从新定位所有对象。现在 `WindowController` 和 `ViewController` 在可视编辑器里显示为相同大小了。选择 `WindowController` 中的窗口，在属性检查器了将它的标题改为 Egg Timer。

将` Autosave` 命名为 `EggTimerMainWindow`，这样在启动时自动保存窗口的大小和位置。

![image.png](https://upload-images.jianshu.io/upload_images/323996-f59212aef30c3abf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


> 如果你是一个 iOS 程序员，你还将处理各种不同的设备类型和旋转。在 macOS 编程中，你必须处理无限的窗口大小和高宽比。这是为什么我把窗口的初始尺寸弄得有点怪异。幸运的是 Auto Layout 为你负责了所有工作。

### UI 布局第一部分

基本的 UI 由两个 `stack views` 组成。第一个包含剩余时间文本和图片。第二个沿着底部包含 3 个按钮。从按钮开始：

 - 在对象库里搜索 Button
- 拖一个 `Gradient button` 到视图里
- 使用属性检查器将它的图片删除，标题改为 Start
- 字体改为 System 24
![image.png](https://upload-images.jianshu.io/upload_images/323996-5481733e11390525.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/515)

- 扩展按钮让它显示所有文本
- 选中 `Start `按钮，按两下 `Command-D` 复制两份
- 拖出两个按钮，让你能看到他们
- 将新按钮的标题改为 `Stop` 和 `Reset`
- 选中 3 个按钮，选择 `Editor/Embed In/Stack View`

![image.png](https://upload-images.jianshu.io/upload_images/323996-b44b6b84775931eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


为了让按钮填充 `Stack View`，选择 `Stack View `作如下改变：

- Distribution: Fill Equally
- Spacing: 0


选择可视编辑器下面的 `Add New Constraints` 按钮，像下面一样设置左、右、下、高度约束。选择 `Update Frames: Items of New Constraints` 然后点击 `Apply 4 Constraints`。

![image.png](https://upload-images.jianshu.io/upload_images/323996-3b8d102e5724fcd2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

`stack view` 现在放置正确了，但是按钮比 `stack view` 短。在文档大纲里面，按着 CTRL 拖动 `Start` 按钮到 `Stack View`，然后选择 `Equal Heights`。其他两个按钮相同做法。

![image.png](https://upload-images.jianshu.io/upload_images/323996-44b5d74a90ed23ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

现在按钮完全像你希望的那样了。
编译运行 APP。尝试改变窗口大小：按钮紧贴着窗口的底部，大小均匀的随着窗口宽带变化。

![image.png](https://upload-images.jianshu.io/upload_images/323996-5993fdb333b81e49.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

最后在视图检查器里面将 Stop 和 Reset 设置为无效。在计时器开始之前它们是有效的是没有意义的。

#### UI 布局第二部分

第二个 `stack view` 包含剩余时间文本和图片。拖动一个标签到视图里面，将标题设置为 6:00，对齐方式设置为 center。当前的系统实体 （San Francisco）使用等比例间距，对于数字这意味着如果你有一个计时器，数字发生改变时它似乎跳来跳去的。这真的很烦人。

将字体改为 `Helvetica Neue` 避免这种情况，字体大小设置为 100。这会使得文本太大，所有扩展标签直到你能看到它。

![image.png](https://upload-images.jianshu.io/upload_images/323996-cf024529d1e86b38.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)

在对象库的过滤框里输入 `image`，搜索并添加图片控件。这会带出几个可能的结果，你想要的是 `Image View`。将它拖到视图里，放在文本标签下面。

从这里下载[资源](https://link.jianshu.com/?t=https%3A%2F%2Fkoenig-media.raywenderlich.com%2Fuploads%2F2017%2F01%2FEggTimerAssets-1.zip)。解压文件，打开 `Egg Images` 文件夹。在 `Xcode` 里点击项目导航器里的 `Assets.xcassets`。

拖动 6 个图片到资源库中。现在你的 APP 可以使用它们了。因为图片文件名中包含 “@2x”，它们已经自动分配到了每个图片资源的 2x 部分。


![image.png](https://upload-images.jianshu.io/upload_images/323996-9526d6950496fd43.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

回到 `Main.storyboard`，选择你刚才添加的图片视图，在属性检查器里点击 Image 弹出菜单。你能看到你刚才添加的图片和内置图片。选择 `stopped`。

制作第二个 `stack view`：选择剩余时间文本标签和图片视图。选择 `Editor / Embed In / Stack View`。现在你需要配置 `stack view` 填满空闲空间。在可视编辑器底部点击 `Add New Constraints` 按钮添加它们的约束。


![image.png](https://upload-images.jianshu.io/upload_images/323996-db3792ba10c35467.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


`stack view` 按要求扩展了，但是图片仍然太小。选择图片视图，将它的左右约束想下面显示的一样设置为 `Standard Value`。


![image.png](https://upload-images.jianshu.io/upload_images/323996-04ee0b8a93d63de6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

在属性检查器里将 Scaling 设置为 `Proportionally Up or Down`。
编译运行 APP。改变窗口大小，查看所有 UI 元素是否像我们期望的一样改变大小和位置。


![image.png](https://upload-images.jianshu.io/upload_images/323996-e9d74175b87abbd9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

#### 连接 UI 和代码

像在第一部分中学到的一样，你需要设置 `@IBOutlets` 和 `@IBActions` 连接 UI 和代码。对于这个窗口你需要为一下元素设置 `@IBOutlets`：

- 剩余时间标签
- Egg 图片
- 3个按钮


当用户点击按钮时，3 个按钮还需要 `@IBActions `来触发函数。在项目导航中选择 `Main.storyboard`。按着 `Option` 点击 `ViewController.swift`，在辅助窗口中打开它。如果你的空间不够，使用右上角的按钮隐藏工具面板和导航面板。选择 `countdown timer label`，就像在第一部分一样，按着 CTRL 拖到 `ViewController` 类里。将标签名称设为 `timeLeftField`。重复 `egg image view`，名称设为 `eggImageView`。为按钮设置 outlets，名称设为 `startButton`， `stopButton` 和 `resetButton`。

按钮还需要 `@IBActions`。按着 CTRL 拖动 Start 按钮，但是这次连接类型改为 Action。名称设为 `startButtonClicked`。重复为另外两个按钮创建 Action，名称为 `stopButtonClicked` 和 `resetButtonClicked`。如果你想我一样，经常忘记将连接类型改为 `Action`，你将得到两个 `IBOutlets`，而不是 `@IBAction`.。为了移除 `IBOutlets`，先删除 `ViewController` 里面多余的代码行，然后转到连工具面板中的接检测器。在 `Referencing Outlets` 下面你将看到两个条目。点击错误条目旁边的 X 删除它。然后回去，这次记得改变连接菜单 `@IBAction`。

![image.png](https://upload-images.jianshu.io/upload_images/323996-7eb6600c4d8841c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


现在 ViewController 看起来应该像这样：


![image.png](https://upload-images.jianshu.io/upload_images/323996-6ad14c77e975732e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


在本系列的第3部分，你将向这些函数里添加代码，让它们工作。关闭辅助编辑器，重新打开导航和工具面板，如果你关闭了它们。

#### 菜单

在 `Main.storyboard` 里，点击 `menu bar` 或者`Application Scene` 选择它。APP 模板提供了一组默认的菜单，但是，对这个 APP 来说它们大部分是不需要的。探索这些菜单最容易的方法是使用 `Document Outline`。使用展开三角形显示菜单视图和它的内如。

![image.png](https://upload-images.jianshu.io/upload_images/323996-be981751e019c90c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)


菜单的结构是一系列的嵌套的菜单和菜单想。切换到工具面板的身份检测器，这样当你点击它们是能看到它们实际是什么。`Main Menu` 是一个 `NSMenu` 类的实例。它包含一个 `NSMenuItems` 数组：View 就是其中之一。视图菜单条目包含一个子菜单 （NSMenu），这个子菜单包含自己的 `NSMenuItems`。注意 `Separator` 条目只是一个特殊化的 `NSMenuItem`。

第一件事是删除这个 APP 里不需要的菜单。在 Document Outline 里选择 File 菜单，然后 按 Delete 删除它。如果你在视图编辑器里删除它，你只能删除 File 菜单条目里的菜单，因此会在菜单条里留下一个空白。如果发生这种情况，选择空白，再次按 Delete 删除它。

一直删除直到只剩下 `EggTimer`, `Window` 和 `Help`。


![image.png](https://upload-images.jianshu.io/upload_images/323996-c29761ab0d48aa53.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/200)


现在你将添加一个新的菜单，用来模仿 3 个按钮的操作。在对象库里搜索 Menu。记住每个菜单都是从菜单项开始的，拖动一个 Menu Item 到菜单条中，放到 EggTimer 和 `Window` 之间。它将显示为一个蓝色盒子，那是因为它还没有一个带标题的菜单。现在拖动一个 Menu 放到蓝色盒子中。如果你觉得难以定位到蓝色盒子，可以拖动到文档大纲，放到新的 Item 下。新菜单依然没有标题，但是现在它有 3 个条目了。

![image.png](https://upload-images.jianshu.io/upload_images/323996-cd0527f888dbc7d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)


选中菜单的同时（不是菜单项），到属性检查器里将标题改为 Timer。这将为你的新菜单分配一个名字。选择 Item1 将它的标题改为 Start ，可以通过双击修改它，或者在属性检查器里修改。

点击属性检查器里的 `Key Equivalent`，按 `Command-S` 赋予它键盘快捷键。通常 `Command-S` 意思是保存，但是因为你已经删除了 File 菜单，这不会造成冲突，尽管重用通用快捷键为其他目的不是一个好的实践。用同样的方法将第二个条目设置为 Stop，快捷键为 Command-X。第三个条目的标题为 Reset，快捷键为 Command-R。

![image.png](https://upload-images.jianshu.io/upload_images/323996-87b22cdd3d324769.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

在可视化编辑器中，你可以在菜单条的顶部看到 3 个按钮。切换到省份检查器。依次点击每一个按钮，显示它们连接到 `Application、First Responder` 和` AppDelegate`。`First Responder` 通常是当前最前面的 `view controller`，它能收到来自菜单项的动作。
按着 `Option` 点击 `ViewController.swift`，在按钮的 `@IBActions `代码的下面添加以下代码。


```swift
// MARK: - IBActions - menus
@IBAction func startTimerMenuItemSelected(_ sender: Any) {
  startButtonClicked(sender)
}
@IBAction func stopTimerMenuItemSelected(_ sender: Any) {
  stopButtonClicked(sender)
}
@IBAction func resetTimerMenuItemSelected(_ sender: Any) {
  resetButtonClicked(sender)
}
```

这些函数将通过菜单调用，它们再调用按钮动作的函数。你可以直接让菜单项调用按钮的方法，但是我选择这种方式使调试时一系列事件更明显。保存文件，关闭辅助编辑器。

按着 CTRL 键将 Start 菜单拖到代表 First Responder 的橙色方块。将弹出一个巨大的选项清单。输入 “sta”快速滚动到正确的部分，选择 `startTimerMenuItemSelected`。

用同样的方法将 `Stop` 菜单项连接到 `stopTimerMenuItemSelected`，Reset 菜单项连接到 `resetTimerMenuItemSelected`。现在当 `EggTimer` 窗口出现在前面，选择菜单项将会调用这些方法。

然而，3 个按钮不会同时为有效状态，菜单项需要反映这些按钮的状态。这不会在 `ViewController `里发生，因为它不会总是 `First Responder`，所以菜单项将被 `AppDelegate` 控制。

在打开 `Main.storyboard` 和 菜单可见的同时，按着 `Option` 点击项目导航中的 `AppDelegate.swift`。按着 `CTRL `将 `Start` 拖到 `AppDelegate`，赋予 outlet 名称 `startTimerMenuItem`。其他菜单项同样做法，名称分别为 `stopTimerMenuItem` 和 `resetTimerMenuItem`。


![image.png](https://upload-images.jianshu.io/upload_images/323996-789ed511923d05d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


在第3部分，你将编写代码来在需要的时候启用和禁用菜单项，但现在，你需要关闭自动的启用和禁用。通常 APP 会检查当前的 First Responder 是否有一个菜单项对应的方法，如果没有禁用它。这个 APP 你希望自己控制它。选择 Timer 菜单取消属性选择器中的 Auto Enables Items。

#### 偏好窗口

![image.png](https://upload-images.jianshu.io/upload_images/323996-e4bbc906a0a2220d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

`EggTimer` 的主窗口现在看起来不错，但现在它需要一个偏好窗口让用户选择他们需要将鸡蛋煮得多好。
偏好窗口以一个单独的窗口出现，拥有它自己的控制器。这是因为偏好窗口将有自己的默认尺寸，并且不能改变大小。同一个窗口控制器拥有多个视图控制器是有可能的，但是那样它们将共享那个窗口控制器的属性。

打开 `Main.storyboard`，如果辅助编辑器是打开的关闭它，在对象库中搜索 “window”。拖一个 `Window Controller` 到可视编辑器。它将同时创建一个 `View Controller` 来包含它的内容。在窗口里面重新排列它们，让新的窗口控制器靠近菜单条。

打开 EggTimer 菜单，按着 `CTRL 从 Preferences… `拖到新的窗口控制器。在弹出菜单中选择 Show。这将创建一个 segue，这导致无论什么时候用户点击 EggTimer 菜单中的 `Preferences…` 窗口控制器将显示新的视图控制器。


![image.png](https://upload-images.jianshu.io/upload_images/323996-ac86b0a34134fe13.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


新的偏好窗口将显示一个新的 view controller，所以你需要为 view controller 设计一个类。在项目导航里面选择已经存在的 ViewController.swift 文件。这样确保你新建的文件在项目导航中有一个逻辑位置。选择 File/New/File…。


![image.png](https://upload-images.jianshu.io/upload_images/323996-8eadfc9b72593767.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)


选择 `macOS/Cocoa Class` 点击下一步。选择类名 PrefsViewController，为它创建一个子类 `NSViewController`。语言选择 Swift，取消 `Also create XIB file for user interface`。点击下一步创建并保存文件。


![image.png](https://upload-images.jianshu.io/upload_images/323996-49f4c32ae28db845.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)


回到 Main.storyboard，选择新的视图控制器。确保你是选择了 view controller 自己，而不是 view。在文档大纲中选择更容易。在身份检查器里将类设置为 PrefsViewController。


![image.png](https://upload-images.jianshu.io/upload_images/323996-054352961ab12077.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)

选择偏好窗口控制器里的窗口，使用属性检查器将它的标题设置为 Preferences。不要设置 autosave 名称，因为这个窗口每次都要显示在屏幕的中心。取消 Minimize 和 Resize，让窗口大小是固定的。


![image.png](https://upload-images.jianshu.io/upload_images/323996-c361d81e25222362.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)

到尺寸检查器里面将 Content Size 的宽设置为 416，高设置为 214。在初始位置下，从两个下拉菜单里选择 Center Horizontally 和 Center Vertically。


![image.png](https://upload-images.jianshu.io/upload_images/323996-0b28b12d3d87c5d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)

选择 `PrefsViewController` 里的视图，使用尺寸检查器将它的宽设置为 416，高设置为 214。

`PrefsViewController` 将显示一个用来选择预置的时间下拉菜单，和一个选择自定义时间的滑块。每一个都有一个对应的标签，还有两个按钮：Cancel 和 OK。这是一个动态显示已选择时间的标签。


![image.png](https://upload-images.jianshu.io/upload_images/323996-706e8f65394f3a17.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

拖动下面这些控件到视图控制器，像图中一样排列它们：

- Label – 标题设置为 “Preset Egg Timings:”
- Pop Up Button
- Label – 标题设置为 “Custom Egg Timing:”
- Label – 标题设置为 “6 minutes”
- Horizontal Slider
- Push Button – 标题设置为 “Cancel”
- Push Button – 标题设置为 “OK”



因为这个窗口大小不可变，所以不需要设置布局约束——对象总是像你安排的那样出现。拖动对象来定位它，使用蓝色参考线来帮助你。扩展 “6 minutes” 标签的宽度几乎到窗口的右边，因为它可能包含很多文本。双击 Pop Up Button 查看它最初的 3 个条目并设置它们的标题：

- For runny soft-boiled eggs (barely set whites): 3 minutes （松软的煮蛋，几乎没有凝成白色）
- For slightly runny soft-boiled eggs: 4 minutes （轻微松软的煮蛋）
- For custardy yet firm soft-boiled eggs: 6 minutes （这是什么鸟蛋，像奶油冻的软蛋？）



从对象库中拖入俩个 Menu Items，然后是一个分割菜单条，最后是另一个菜单项。如果有定位的问题，可以使用文档大纲。
将剩余的菜单项的标题设置为：

- For firm yet still creamy hard-boiled eggs: 10 minutes （坚固但是有点乳脂状的硬蛋）
- For very firm hard-boiled eggs: 15 minutes（非常硬的蛋）
- Custom（自定义）


![image.png](https://upload-images.jianshu.io/upload_images/323996-2c314ec1aafd7765.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

我不会假装有很好的关于煮蛋的知识， 这些时间和描述来自 The Kitchn.

选择弹出菜单，注意不是菜单项，将 Selected Item 设置为 6 分钟那个选项。

现在你要做一个非常巧妙的技巧，让 APP 准确的知道选择了几分钟。在视图检查器里将每一个弹出菜单项的 tag 设置为数字：3, 4, 6, 10, 15。Custom 菜单项保留为 0。


![image.png](https://upload-images.jianshu.io/upload_images/323996-548a7c3d33429080.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/400)

现在选择滑块，在属性检查器里将 Tick marks 设置为 25，Minimum Value 设置为 1，Maximum Value 设置为 25，Current Value 设置为 6，选中 Only stop on tick marks。你可能想要将滑块向下移动几个像素。通过不选择 Enabled 来禁用它——它只有在弹出菜单中 Custom 被选择时才启用。


![image.png](https://upload-images.jianshu.io/upload_images/323996-166cdaa33d33d403.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)

#### 连接偏好对象

按着 Option 点击项目导航中的 PrefsViewController.swift，如果需要空间隐藏侧边的面板。你需要弹出菜单、滑块和标签的 @IBOutlets。按着 CTRL 将它们拖到 PrefsViewController，将名称设置如下：

- Popup: presetsPopup
- Slider: customSlider
- Label: customTextField


下一步，通过按着 CTRL 拖动来创建 @IBActions，记得每次都在连接菜单中选择 Action：

- Popup: popupValueChanged
- Slider: sliderValueChanged
- Cancel button: cancelButtonClicked
- OK button: okButtonClicked


你的代码现在看起来应该像这样：


![image.png](https://upload-images.jianshu.io/upload_images/323996-c5d0829df459f2e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


现在偏好窗口的布局完成了。编译运行 APP，从 EggTimer 菜单中选择 Preferences 。看完偏好窗口后点击标题栏中的红色按钮关闭窗口。


![image.png](https://upload-images.jianshu.io/upload_images/323996-2e90d58975f5a24a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


#### App 的图标

现在 UI 设计只剩下最后一部分——为 APP 添加一个图标了。你已经下载了一个资源文件夹，并安装了一部分图片到 Assets.xcassets 了。再次打开文件夹找到 egg-icon.png 文件。

在项目导航中选择 `Assests.xcassets`。点击 AppIcon。将 egg-icon.png 拖到 Mac 256pt 1x 盒子中。就像在第一部分中讨论的一样，对于一个产品 APP，你应该提供 AppIcon 中显示的所有尺寸，但是在这个 APP 中单独提供一个就足够了。


![image.png](https://upload-images.jianshu.io/upload_images/323996-6e28b97c5028d6af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

编译运行 APP，确认新的图标出现在 Dock 中。如果你仍然看见默认图标，选择 Product 菜单中的 Clean 再试试。

现在你已经完全实现了 APP 的 UI，但是 APP 还没有做任何事情。如果你在任何地方感到迷惑，可以从 [the Xcode project](https://link.jianshu.com/?t=https%3A%2F%2Fkoenig-media.raywenderlich.com%2Fuploads%2F2017%2F01%2FEggTimer-Part2-3.zip) 下载全部 UI 实现，为下一部分做准备。

在这个系列的第3部分，你将添加代码让 APP 工作起来。
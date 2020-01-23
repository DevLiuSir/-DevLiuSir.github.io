---
layout: post
title:  马上着手开发 iOS 应用程序 (四) - 连接 UI 和代码
date:   2016-04-01 22:27:49
categories: Swift
tags: Swift
---

<h2>马上着手开发 iOS 应用程序 (四) - 连接 UI 和代码</h2>


翻译自苹果官网:

[Connect the UI to Code](https://developer.apple.com/library/prerelease/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Lesson3.html#//apple_ref/doc/uid/TP40015214-CH22-SW1)


在本课中，连接 FoodTracker app 的 UI 和源代码并且定义一些用户可以在 UI 中执行的动作。当完成这些，你的 app 看起来是这样：

![](https://developer.apple.com/library/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Art/3_sim_finalUI_2x.png)

<h3>学习目标</h3>

在课程的最后，你将学会：

* 解释` storyboard `中的场景和底层的视图控制器的关系。
* 在` storyboard` 中的 UI 控件和源代码之间创建` outlet` 和` action` 连接
* 处理文本框中的用户输入及在 UI 中显示结果
* 创建一个类遵循协议
* 理解代理设计模式
* 当设计 app 结构时遵循 target-action 模式

<h3>连接 UI 和源代码</h3>

storyboard 中控件关联着源代码。理解 storyboard 和你写的代码间的关系非常重要。

在 storyboard 中，一个`场景`代表一个屏幕的内容通常是个视图控制器。视图控制器实现 app 的功能并管理内容视图和它的各级子类，同时协调封装数据的模型层和显示数据的视图层之间的信息流，管理它们的内容视图的生命周期，当设备旋转时处理方向变化，在 app 中定义导航，响应用户输入。所有的 iOS 视图控制器对象都是` UIViewController `和它的子类型。

通过在代码中创建和实现自定义控制器类来定义功能。在这个类和` storyboard `场景间创建连接,这样你就同时得到了代码中的功能和` storyboard` 中的 UI。

Xcode 已经在之前创建了这样的类了，` ViewController.swift `,把它连接到之前定义的场景。未来，你会遇到更多的场景，那时你得自己在识别检查器`（Identity inspector）` 中连接它们。识别检查器让你能够编辑 storyboard 中对象的与身份相关属性，例如对象属于哪个类。

![](https://developer.apple.com/library/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Art/3_inspector_identity_2x.png)


运行时，storyboard 会创建自定义 ViewController 的实例，app 显示 storyboard 中定义的 UI 并执行` ViewController.swift `中定义的一些功能。

尽管场景成功连接到了` ViewController.swift`，但这不是唯一需要连接的。为了在 app 中定义交互，控制器源代码需要与 storyboard 中视图关联。通过在视图和控制器源代码间定义 outlets 和 acitons 来实现关联。

<h3>创建 UI 控件Outlets</h3>

`Outlets `是在代码中引用界面对象的一种方式。为了创建` Outlet`，按住` Control` 从 `Storyboard `中对象往控制器文件拖拽。这会在控制器中创建对象属性，让你能在运行时访问这个属性。

你需要为 UI 中文本框和标签创建 `outlets` 来引用它们。

<h4>连接文本框和 ViewController.swift 代码</h4>

1.打开` Main.storyboard`。

2.点击 Xcode 右上角工具栏中的` Assistant` 按钮来打开 `辅助编辑器 `。

![](https://developer.apple.com/library/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Art/assistant_editor_toggle_2x.png)

3.如果你想要更多空间来工作，请点击 Xcode 工具栏中的` Navigator `和` Utilities `按钮来收缩项目导航和实用工具区。

![](https://developer.apple.com/library/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Art/navigator_utilities_toggle_on_2x.png)

同样可以收缩`大纲视图`

4.在辅助编辑器顶部的编辑器选择栏，从` Preview `修改辅助编辑器为` Automatic` > `ViewController.swift `。

![](https://developer.apple.com/library/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Art/3_switchtoviewcontroller_2x.png)

`ViewController.swift `会在右边的编辑器中显示。


5.在` ViewController.swift `中，找到如下的` class `行：

 class ViewController: UIViewController {

6.在` class `行下面，添加如下行:

>	// MARK: Properties
 
这仅仅向源代码添加了一行`注释`。回忆一下，`注释 `是代码中不会被编译的文本，但提供了有关代码的上下文说明信息。

以` // MARK: `开头的的注释是种很特别的注释，让你（或任何其他读你代码的人）组织代码并进行导航的。你稍后会在 action 一节中再次看到它的。具体来说，用它来分割逻辑代码区域。

7.在` storyboard` 中，选择`文本框`。

8.按住 Control 从画板中的文本框拖动到右边编辑器中的代码。在刚刚添加的注释下面停止拖动。ViewController.swift.

![](https://developer.apple.com/library/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Art/3_textfield_dragoutlet_2x.png)


9.在打开的对象框中，输入名字` nameTextField`。
忽略其他的选项。最后对话框应该像这样:

![](https://developer.apple.com/library/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Art/3_textfield_addoutlet_2x.png)



10.点击 Connect


>Xcode 向 ViewController.swift 中添加必要的代码来存储文本框的指针并且配置 storyboard 来设置刚才的连接。

>	@IBOutlet weak var nameTextField: UITextField!

花点时间理解这行代码发生了什么。

`IBOutlet` 属性告诉 Xcode 让你可以从 ` Interface Builder `中连接` nameTextField `属性(这就是为什么属性有 IB 前缀)。` weak `关键字意味着属性在生命周期某些时候可能没有值(是 nil)。剩下的部分定义了一个叫` nameTextField` 的文本框。

注意在声明的结尾` ! `。你可能还记得在某些类型的AppDelegate.swift文件的末尾看到这些。这个感叹号`(!) ` 表明该类型是` 隐式解包可选`的，这是一个可选类型，在第一次赋值后就一直存在值.

现在，以同样的方式连接` TextFiled`和 你的代码。

连接` TextFiled `和 `ViewController.swift`代码

1.在 `storyboard `中，选择` TextFiled `。
按住` Control `从画板中的标签拖动到右边编辑器的代码中，在` ViewController.swift `中 `nameTextField `属性的下面的行停止拖动。


![](https://developer.apple.com/library/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Art/3_label_dragoutlet_2x.png)


3.在出现的对话框中，输入名字` mealNameLabel`。忽略其他选项。你的对话框应该是这样：

![](https://developer.apple.com/library/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Art/3_label_addoutlet_2x.png)


4.点击` Connect`.

再一次，Xcode 往 `ViewController.swift `中添加了必要的代码来存储标签的指针并配置 storyboard 来设置连接。这个` Outlet `除了名字和类型其他都类似之前文本框的。


再次，Xcode 以 `ViewController.swift`中添加必要的代码，来存储一个指向 和配置故事板来设置该连接。该类似 `TextFiled`，除了它的名字和它的类型（这是一个`UILabel`，以匹配 storyboard）。

> 	@IBOutlet weak var mealNameLabel: UILabel!
	
当使用这种方式成功在代码中引用界面控件，你需要定义事件来与这些控件触发交互。这就是 

现在，你有办法从代码中引用界面元素控件，你需要定义一个事件触发这些元素之间的互动。这就是Actions 的由来。


<h3>定义一个动作(Action)来执行</h3>


iOS app 是基于` 事件驱动的程序 `。那就是，app 的流程是由系统事件和用户动作决定的。用户在界面中执行动作并触发事件。这些事件导致 app 逻辑的执行和相应的数据处理。app 响应用户动作并反馈到 UI。当确定代码块执行时，因为 app 是用户而不是开发者在控制的，所以在代码中标示用户可以执行哪个动作以及怎么响应这些动作。

`动作`（或动作方法）是 app 中关联事件的一部分代码块。当事件发生了，对应代码得到执行。你可以定义一个动作方法来完成从操作一块数据到更新 UI 的任何事情。使用动作驱动 app 的流程来响应用户或系统的事件。

像创建 outlet 一样创建 action：按住 Control 从一个 storyboard 中特定对象向视图控制器文件拖动。这步操作会在视图控制器中创建一个方法，当用户与这个方法关联的对象交互时就会触发这个方法。

通过创建一个简单的动作:当用户点击` Set Default Label Text `按钮，设置标签为默认值,`Default Text`（设置标签为文本框中文本的代码有点复杂，所以将在下一节讲解。）


<h3>在 ViewController.swift 代码中创建标签重置动作</h3>

1.在 ViewController.swift 的最后一个大括号(}) 前面，添加如下注释:

>	// MARK: Actions
 	
这行注释表明以下是写 Actions 的区域。

2.在 storyboard 中， 选择` Set Default Label Text `按钮。

3.按住` Control `从画板中的` Set Default Label Text `按钮拖动到右边编辑器代码中，在刚才的注释行下面停止拖动。

![](https://developer.apple.com/library/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Art/3_button_dragaction_2x.png)


4.在弹出的对话框中 `Connection `那一行选择` Action`。

对于名称，类型设置默认标签文本。
5.对于名称，类型设置 Name 那一行输入: setDefaultLabelText 。
6.对于类型，Type 那一行，选择` UIButton`。

你或许注意到文本框的默认值` AnyObject`。在 Swift 中， `AnyObject `类型用于描述属于任何类的对象。指定` action `方法的类型为` UIButton `意味着` 只有按钮对象可以连接这个 action。`尽管对现在创建的这个动作没有什么意义，但是记住这点很重要。

忽略剩下的选项。你的对话框应该是这样：

![](https://developer.apple.com/library/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Art/3_button_addaction_2x.png)


7.点击 Connect。

Xcode 向 `ViewController.swift `中添加必要的代码来设置 action 方法。

	@IBAction func setDefaultLabelText(sender: UIButton) {
	}


`sender `参数:指向负责触发动作的对象-当前是这个按钮。 `IBAction `属性表明这个方法是个 `action` 可以在 storyboard 中连接这个方法。剩下的定义就是方法名字 `setDefaultLabelText(_:)` .

方法实现是空的。设置标签值为空非常简单。


<h3>在 ViewController 代码中实现重置标签文本动作</h3>

1.在` VieController.swift `中，找到刚才添加的`setDefaultLabelText `动作方法。
在大括号（{}）之间的代码实现中，添加这行代码:
	
	mealNameLabel.text = "Default Text"
 
或许你会猜到，这句代码就是设置` Label `的文本为默认值。

注意到你没有指定` Default Text` 的类型，因为 Swift 类型推断发现你给标签赋值 String 类型的值所以可以正确推断类型。

iOS 系统为你默认执行了所有的重绘操作，所以你只需写这句代码就行了。你的 setDefaultLabelText(_:) 方法最后应该是这样:

	@IBAction func setDefaultLabelText(sender: UIButton) {
	    mealNameLabel.text = "Default Text"
	}



检验: 运行模拟器测试你的修改。当点击` Set Default Label Text `按钮，标签的文本应该从 `Meal Name `(storyboard 中设置的值) 变为` Default Text `(动作设置的值)。

![](https://developer.apple.com/library/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Art/3_sim_defaulttext_2x.png)


你刚才的行为是 iOS app `target-action `设计模式的一个例子。当特定的事件发生相应对象就会发送消息给另外一个对象。在这个例子中，事件就是用户点击` Set Default Label Text `按钮，动作就是 `setDefaultLabelText`, `target` 就是` ViewController`，`sender `就是` Set Default Label Text `按钮。消息就是在源代码中定义的动作方法，target 就是接受消息并有能力执行动作的对象。发送动作消息通常是 control 对象，例如按钮，slider 或 switch - 它们可以触发事件来响应用户的交互如点击，拖动或值改变。这种设计模式在 iOS app 开发中很常见，后面的课程你会看到更多的例子。

***

<h3>处理用户输入</h3>

使用这种方式重置` Label `的默认值，继续添加一个功能来设置` Label `为文本框中的值。我们简单处理，当用户点击文本框键盘上的` Return `按钮，更新` Label `的值。

当使用一个`文本框`来接受用户输入，你需要一个`文本框代理`的帮助。一个 `delegate `代表一个对象去执行一些任务。代理对象 - 在本例中是这个文本框会保持代理的引用。在合适的时间它会发送消息给代理。告诉代理它将要处理或刚刚已经处理过的事件。代理可能做一些响应如更新它自己或 app 中其他对象的外观和状态，或者返回值来影响即将到来的事件的处理。

只要遵循合适的协议任何对象都可以作为代理为另一个对象服务。文本框的代理协议叫做 
` UITextFieldDelegate。`在这个例子中，因为` ViewController `保持了文本框的引用，设置` ViewController `为文本框的代理。

通过把它列在类定义的后面让 `ViewController` 遵循 `UITextFieldDelegate` 协议。

<h3>遵循 UITextFieldDelegate 协议</h3>


1.	在打开的辅助编辑器中，点击` Standard `按钮返回 标准的编辑器。

![](https://developer.apple.com/library/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Art/standard_toggle_2x.png)


点击 Xcode 工具栏的` Navigator `和 `Utilities` 按钮来展开项目导航和实用工具区。

2.	在项目导航中，选择` ViewController.swift`。

3.	在` ViewController.swift `中，找到如下的 `class `行:

	>	class ViewController: UIViewController {
 
 
 
在` UIViewController `后面添加一个` , `和` UITextFieldDelegate `来遵循这个协议。

	class ViewController: UIViewController, UITextFieldDelegate {
 
通过遵循这个协议，`ViewController `就有能力确认自己是` UITextFieldDelegate`。

意味着你可以设置它作为`文本框的代理`并实现处理用户文本框输入的功能。


<h3>设置` ViewController` 作为 `nameTextField `的代理</h3>


1.在` ViewController.swift `中，找到如下的` ViewDidLoad() `方法：

	 override func viewDidLoad() {
	     super.viewDidLoad()
	     // Do any additional setup after loading the view, typically from a nib.
	}
 
模板方法的实现包含一行注释。我们不需要它，所以删掉。

2.在` super.viewDidLoad() `行下面，添加一个如下代码：

	 // Handle the text field’s user input through delegate callbacks.
	 nameTextField.delegate = self
	 
`self `指的是` ViewController 类`，因为它是在` ViewController `类的定义范围内引用的。

可以添加自己的注释来帮助理解代码的逻辑。

`viewDidLoad() `方法应该像这样：

	override func viewDidLoad() {
	    super.viewDidLoad()

	    // Handle the text field’s user input through delegate callbacks.
	    nameTextField.delegate = self
	}


`ViewController `现在是` nameTextField` 的一个代理了。

`UITextFieldDelegate `协议包含`可选`方法，意味着它们不是必须要实现的。但是为了得到你想的特定的行为，需要实现它们中的两个方法:

	func textFieldShouldReturn(textField: UITextField) -> Bool
	func textFieldDidEndEditing(textField: UITextField)


为了理解什么时候调用这些方法，首先需要知道文本框怎么响应用户事件。当用户点击一个文本框，文本框自动变成第一响应者。而` 第一响应者` 是对象集合中第一个接收 app 事件的对象，事件包括键盘事件，移动事件和动作消息，当然还有其他的。换言之，用户的事件优先传给第一响应者。

当文本框变成第一响应者时，iOS 会显示键盘以及一个编辑区域。让用户使用键盘而不是文本框输入文字。

当用户完成编辑，文本框应该放弃第一响应者状态。点击 Return(本例是 Done) 按钮来结束在文本框中的编辑,相应的 textFieldShouldReturn(_:) 方法触发了

<h3>实现` UITextFieldDelegate` 协议方法` textFieldShouldReturn(_:)` </h3>


1.在` ViewController.swift `文件中 // MARK: Actions 的右上角，添加如下注释:

>	// MARK: UITextFieldDelegate
 
这行注释是用来组织代码并帮助你（或任何其他读你代码的人）导航。

到目前为止添加了一些注释。Xcode 在` Functions menu` 区域列举了每个注释,点击标题来跳到对应的区域。

![](https://developer.apple.com/library/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Art/3_functionsmenu_2x.png)


2.在注释的下面，添加如下方法:

	 func textFieldShouldReturn(textField: UITextField) -> Bool {
	 
	 }
	 
	 
3.在这个方法中，添加如下代码来放弃文本框的第一响应者状态，并添加注释描述代码做了什么:

	 // Hide the keyboard.
	 textField.resignFirstResponder()
	 
 
不要复制粘贴而是自己输入第二行的代码。当输入时` Xcode `会弹出代码提示列表，滚动这个列表找到你想要的那项然后回车确认选择。Xcode 代码提示真是非常牛逼和省时的功能。

![](https://developer.apple.com/library/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Art/3_code_completion_2x.png)

4.在方法中，添加如下代码行。

	return true
 
由于该方法返回一个布尔值 true 表示文本框能够响应用户点击` Return `键来关闭键盘的操作。

你的 `textFieldShouldReturn(_:) `方法最后应该像这样:

	func textFieldShouldReturn(textField: UITextField) -> Bool {
	    // Hide the keyboard.
	    textField.resignFirstResponder()
	    return true
	}
	
第二个需要实现的方法是` textFieldDidEndEditing(_:)`,在文本框放弃第一响应者状态时被调用。也就是在你刚实现的` textFieldShouldReturn `后面。

你可以在` textFieldDidEndEditing(_:) `方法中读取刚才输入到文本框中的文本信息。在这个例子中，你会得到文本框的文本并使用它来修改标签的值。

<h3>实现 UITextFieldDelegate 协议的方法 textFieldDidEndEditing(_:)</h3>

1.在 ViewController.swift 的 textFieldShouldReturn(_:) 方法后面，添加如下方法:

	 func textFieldDidEndEditing(textField: UITextField) {
	 }
	 
	 
2.在方法中，添加如下代码:
	
	mealNameLabel.text = textField.text
	
最后 `textFieldDidEndEditing(_:) `方法应该像这样:

	func textFieldDidEndEditing(textField: UITextField) {
	    mealNameLabel.text = textField.text
	}

检验: 运行模拟器测试你的修改。选择`文本框`并输入文本。当你点击键盘上的完成按钮，键盘消失然后会在标签中显示文本框中输入的文本。当点击` Set Default Label Text` 按钮，Label从当前显示的文本变为 `Default Text`。

![](https://developer.apple.com/library/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/Art/3_sim_finalUI_2x.png)


注意
为了查看本课的完整实例项目，下载文件并在 Xcode 中查看它。

[下载文件](https://developer.apple.com/sample-code/swift/downloads/Start-Dev-iOS-Apps-03.zip)
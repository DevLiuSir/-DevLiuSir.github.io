---
layout: post
title:  设置Xcode自动生成代码片段
date:   2015-08-08 22:21:49
categories: 能工巧匠集
tags: 能工巧匠集
---

### 在各种程序开发中，编写代码的效率是非常重要的一个问题，各种优秀的编译器也都有相应的插件用于提高程序员的编码速度。在`xcode`中，可以通过定义代码块的方式将常用的一部分代码进行快捷呼出，首先xcode中为我们定义好了许多代码块，例如：


## 1.什么是代码片段

- 当在Xcode中输入`for in` 并回车后，Xcode会出现下图所示的提示代码：

![](http://ww1.sinaimg.cn/large/c3a20316gw1f4a3efljcjj20ls05imxc.jpg)

这就是代码片段，目的是使程序员以最快的速度输入常用的代码片段，提高编程效率。

![](http://static.oschina.net/uploads/space/2015/0813/093022_1OKK_2340880.png)

在使用这些代码模板时，我们只需要将其拖入我们的代码中，或者直接键入快捷键即可，十分的方便，当然，我们也可以将我们自己常用的代码块包装成模板，便于我们复用。

例如，我写如下的代码段：

```swift
for item in items {
    code
}

```

将其选中，长按左键，直到鼠标变成箭头模样，然后将其拖入右下方的代码块模板区，会弹出如下设置菜单：

![](http://ww1.sinaimg.cn/mw690/c3a20316gw1f4a3k8l129j20zq0gegov.jpg)

## 编辑代码片段
经过第2步操作，松开鼠标左键的同时，会弹出代码片段编辑窗口，如下图所示：
![](http://ww2.sinaimg.cn/mw690/c3a20316gw1f4a3qif5g8j20nk0g040q.jpg)


 - `title`用来设置我们的代码块名称。

 - `platform`用来设置代码块可以使用的平台。

 - `Language`用来设置支持的编程语言。

 - `Completion Shortcut` 代码片段的快捷方式，比如本文开头用到的dowhile，在这里，把属性设置的快捷方式设为 for in
 - `Completion Scopes`  可以在哪些文件中使用当前代码片段，比如全部位置，头文件中等，当然可以添加多个支持的位置。


最后的一个大得空白区域是对代码片段的效果预览。

我们点击Done后，在代码中输入pro会自动提示出这段代码段，变成方便了许多。


- 代码片段的备份
 - Xcode中的代码片段默认放在下面的目录中：

```swift 	
~/Library/Developer/Xcode/UserData/CodeSnippets   
```
我们可以将目录中的代码片段备份，也可以将其直接拷出来放在不同的电脑上使用，因此多台电脑之间的协作也毫无压力。
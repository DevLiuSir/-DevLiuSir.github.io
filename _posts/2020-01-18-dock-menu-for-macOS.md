---
layout: post
title: macOS 开发之 添加Dock菜单
date: 2020-01-18 22:21:49 +0900
categories: [能工巧匠集, macOS]
tags: [swift, DockMenu, macOS开发]
---

![](/assets/images/2020/DockMenu.png)


#### 如图所示，添加Dock菜单，为了方便常用按键在dock上显示。比如QQ音乐、网易云mac端等等。

- 在 `AppDelegate` 中实现`func applicationDockMenu(_ sender: NSApplication) -> NSMenu?`方法即可。代码如下：


```swift

func applicationDockMenu(_ sender: NSApplication) -> NSMenu? {
	let dockMenu = NSMenu()
    //添加Item 显示标题和点击响应事件以及快捷键，快捷键可以为空或者“”
    dockMenu.addItem(withTitle: "播放", action: #selector(play), keyEquivalent: "Play")
    dockMenu.addItem(withTitle: "暂停", action: #selector(stop), keyEquivalent: "Stop")
    return dockMenu
}

func play() {
    print("play")
}

func stop() {
    print("stop")
}
```

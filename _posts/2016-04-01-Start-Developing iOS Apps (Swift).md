---
layout: post
title:  马上着手开发 iOS 应用程序 (一) -现在就开始
date:   2016-04-01 22:22:49
categories: Swift
tags: Swift
---


<h2>马上着手开发 iOS 应用程序 (一) -现在就开始</h2>


> * 重要：这是针对于正在开发中的API或技术的预备文档(预发布版本)。
	苹果提供这份文档的目的是帮助你按照文中描述的方式对技术的选择及界面的设计开发进行规划。
	这些信息有可能发生变化，因此根据本文档的软件开发应当基于最终版本的操作系统和文档进行测试。
	该文档的新版本或许会随着API或相关技术未来的发展而进行更新。

翻译自苹果官网:

[Start Developing iOS Apps (Swift) - Jump Right In](https://developer.apple.com/library/prerelease/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift/index.html#//apple_ref/doc/uid/TP40015214-CH2-SW1)


`马上着手开发 iOS 应用程序（Swift）`是创建能运行在 `iPhone` 和 `iPad `上应用程序的完美起点。查看这一系列循序渐进的课程来指导你构建你的第一个 app - 包括工具，主要概念和最佳实践让你的学习之路更加简单。

每个课程包含一篇教程以及一些概念信息，你需要完成它们。课程间彼此互相关联，让你经历逐步创建一个简单，真实世界的 iOS app 的过程。

当你经历构建 app 的过程，你将了解 iOS app 开发的概念，增加对 Swift 开发语言的理解，并熟悉 Xcode 这个苹果集成开发环境（IDE）的许多有价值功能。

<h3>关于课程</h3>

在这些课程中，主要学习开发一个叫 `FoodTracker `的食物跟踪 app。
这个 app 展示由食物名称、评分及照片组成的列表。
用户可以增加一份新的食物，也可以删除或编辑现有的一份。
为了完成这些操作,用户需要导航至不同的界面，在那里可以为特定食物指定名字、评分和照片。

![](http://upload-images.jianshu.io/upload_images/909589-19ba58cf7e9e393f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



第一节课是` playground` ，它是种能让你与代码进行交互并实时看到结果的 Xcode 文件。在` Xcode `中打开它，并用它来熟悉 `Swift` 的关键概念。

剩下的每节课程都伴随着一个 Xcode 项目，展示课程结束时你的代码和界面应该是什么样子。当学完一节课程，你可以下载这个项目并对照它检查你的学习情况。

最后有个词汇表帮助你回忆在整个课程中学到的概念，词汇表关联所有这些课程。

<h3>获取开发工具</h3>

为了使用这些课程中描述的最新技术来开发` iOS` 应用程序，你需要一台 Mac 电脑（OS X 10.10 或之后）运行最新版本的 Xcode。 Xcode 包括所有设计、开发和调试 app 需要的工具。它同样包含` iOS SDK`。iOS SDK 扩展了 Xcode 的功能，它包含 iOS 开发专用的`工具`、`编译器`和`框架`等。

免费从 App Store 下载最新版本的 Xcode 到你的 Mac 上。

<h3>下载最新版本 Xcode</h3>

*  1、 打开 Mac 上的 `App Store` 程序（默认位于 Dock 中）。
*  2、 在右上角的搜索栏中，输入 Xcode，回车确认。Xcode 程序显示在第一个搜索结果。
*  3、 单击“获取”，然后点击安装程序。
*  4、 当提示时输入你的 Apple ID 和密码。Xcode 会下载并安装到到你的应用目录中
	
><h3>重要提示：</h3>
课程使用 Xcode 7.0 和 iOS SDK 9.0。确保使用了最新版本的` Xcode` 和 `iOS SDK`
当一切准备完毕，让我们开始吧！
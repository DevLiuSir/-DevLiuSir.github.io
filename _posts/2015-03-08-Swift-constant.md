---
layout: post
title: Swift语言基础之常量
date: 2015-03-08 22:26:49 +0900
categories: [能工巧匠集, Swift]
tags: [iOS, Swift]
---


> * 常量一旦设定，在程序运行时就无法改变其值。
* 常量可以是任何的数据类型如：`整型常量`，`浮点型常量`，`字符常量`或`字符串常量`。同样也有`枚举类型的常量`：
* 常量类似于变量，区别在于常量的值一旦设定就不能改变，而变量的值可以随意更改。

<h2>常量声明</h2>

常量使用关键字 `let` 来声明，语法如下：

```swift
let constantName = <initial value>
```

以下是一个简单的 Swift 程序中使用常量的实例：

```swift
let constA = 42
print(constA)
```

以上程序执行结果为：
	
	42
	
<h3>类型标注</h3>

>当你声明常量或者变量的时候可以加上类型标注（type annotation），说明常量或者变量中要存储的值的类型。如果要添加类型标注，需要在常量或者变量名后面加上一个冒号和空格，然后加上类型名称。

```swift
var constantName:<data type> = <optional initial value>
```

以下是一个简单是实例演示了 Swift 中常量使用`类型标注`。需要注意的是常量定义时必须初始值：

```swift
let constA = 42
print(constA)	

let constB:Float = 3.14159
print(constB)
```

以上程序执行结果为：

	42
	3.14159

<h3>常量命名</h3>

> * 常量的命名可以由`字母`，`数字`和`下划线`组成。
* 常量需要以`字母`或`下划线`开始。

`Swift` 是一个区分大小写的语言，所以字母大写与小写是不一样的。

*常量名也可以使用简单的 `Unicode` 字符，如下实例：*

```swift
let _const = "Hello, Swift!"
print(_const)

let 你好 = "你好世界"
print(你好)
```

以上程序执行结果为：

	Hello, Swift!
	你好世界

<h3>常量输出</h3>

>变量和常量可以使用 `print` 函数来输出。

在字符串中可以使用括号与反斜线来插入常量，如下实例：

```swift
let name = "Apple"
let site = "http://www.apple.com"
print("\(name)的官网地址为：\(site)")
```

以上程序执行结果为：

```swift
Apple的官网地址为：http://www.apple.com
```

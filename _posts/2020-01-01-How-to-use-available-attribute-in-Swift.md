---
layout: post
title: 如何在Swift中使用(#available属性)
date: 2020-01-01 22:21:49 +0900
categories: [能工巧匠集, Swift]
tags: [iOS, Swift, macOS]
---

- 在不断变化的应用程序开发环境中，需要将代码段标记为每个平台或版本可用。当新的Swift版本或平台版本到来时，我们希望尽快采用它。在不放弃对旧版本的支持的情况下，我们可以利用Swift中的`available`属性。


- 这篇文章不会帮助您确定确定应该支持的最低iOS版本的策略，但是对于那些需要使用较新API的情况，这将是一个有用的参考。

## 检查操作系统版本以执行代码
一个基本示例归结为检查特定的OS版本以执行一段代码。例如，如果您只想在iOS 14及更高版本上执行一段代码，则可以按以下方式使用 `available`属性：


```swift
if #available(iOS 14, *) {
    print("This code only runs on iOS 14 and up")
} else {
    print("This code only runs on iOS 13 and lower")
}
```


您也可以在 `guard` 语句中使用 `available` 属性：


```swift
guard #available(iOS 14, *) else {
    print("Returning if iOS 13 or lower")
    return
}
print("This code only runs on iOS 14 and up")
```


对于只想为特定iOS版本执行特定代码的情况，这非常有用。


## @available和#available之间的区别
浏览`Swift API`时，您经常会碰到该`@available`属性。我们刚刚介绍了`#attribute`相似但略有不同的。描述其区别的最短答案：

- `@available`: 用于标记类或方法的可用性

- `#available`: 用于仅针对特定平台和/或版本执行一段代码


## 设置类或方法的可用性
我们可以通过将类或方法标记为自iOS 14开始可用来证明这一点：

```swift
@available(iOS 14, *)
final class NewAppIntroduction {
    // ..
}
```

一旦尝试在支持低于iOS 14版本的项目中创建此类的实例，就会遇到以下错误：

![](/assets/images/2020/available_swift.png)

>如果创建的实例不可用，则会发生错误。


如您所见，编译器将通过一些建议帮助我们修复此错误。通过将调用类也标记为自iOS 14开始可用，其中两个将把问题移到代码中的其他位置。`#available`在这种情况下，它将帮助我们：

```swift
if #available(iOS 14, *) {
    let appIntroduction = NewAppIntroduction()
} else {
    // .. use the old app introduction
}
```


我们可以使用`@available`属性对方法进行完全相同的操作：

```swift
@available(iOS 14, *)
func launchNewAppIntroduction() {
    let appIntroduction = NewAppIntroduction()
}
```

## 可用属性的可能值
在以上示例中，我们仅使用了iOS 14检查。但是，我们可以使用Swift中的`available`属性做更多的事情。

显然，我们可以用任何可用的OS版本替换数字14。该平台（在本例中为iOS）也可以替换。截止到今天，可用属性列表如下：

- iOS
- iOSApplicationExtension
- macOS
- macOSApplicationExtension
- macCatalyst
- macCatalystApplicationExtension
- watchOS
- watchOSApplicationExtension
- tvOS
- tvOSApplicationExtension
- swift


选项包括平台以及将`swift`代码段标记为自特定`Swift`版本以来可用的密钥。

除非特别说明，星号表示上面列出的所有平台名称上声明的可用性。如果需要，您也可以一次指定多个平台：


```swift
@available(iOS 14, macOS 11.0, *)
func launchNewAppIntroduction() {
    let appIntroduction = NewAppIntroduction()
}
```


## 将方法标记为已弃用或过时
另一个属性值用于将方法标记为已弃用或已废弃。方法从不推荐使用开始，最终将被标记为已废弃。想象一下，有一个应用程序介绍将不再在iOS 14及更高版本上显示。如果在SDK中使用了特定的方法，则可以将其标记为已弃用和过时，如下所示：

```swift
@available(iOS, deprecated: 12, obsoleted: 13, message: "We no longer show an app introduction on iOS 14 and up")
func launchAppIntroduction() {
    // ..
}
```

当我们的代码的实现者仍然尝试使用我们的方法时，它们将遇到以下错误：

![](/assets/images/2020/deprecated_swift.png)

> 在Swift中将方法标记为不推荐使用。




使用这些值时的版本编号通常会造成混淆。在上面的代码示例中，您可能会认为该方法在iOS 12上已被弃用，在iOS 13中已被废弃。但是，其含义不同：

- 在高于X的版本中不建议使用此方法
- 在高于X的版本中此方法已废弃

该消息用于描述推理，并且有助于向实现者说明更改。

## 将方法标记为重命名
在为同事，开源或其他用户开发SDK时，您可能希望将实现者迁移到代码的较新方法。在这些情况下，您可以使用重命名的属性：

```swift
@available(*, unavailable, renamed: "launchOnboarding")
func launchAppIntroduction() {
    // Old implementation
}

func launchOnboarding() {
    // New implementation
}
```

请注意，我们首先将方法标记为不可用。该`renamed`值指示应改用哪种方法。

Xcode通过显示用于重命名的修复选项将很好地帮助实现者：

![](/assets/images/2020/renaming_method_swift.png)


## 否定可用语句
使用`available`属性时，一个常见的问题是否定该语句并编写如下代码：

> 仅在iOS版本低于X时运行此命令

从今天开始，唯一的方法是按如下所示反转可用的检查：

```swift
if #available(iOS 14, *) { } else {
    // Run iOS 13 and lower code.
}
```


## 结论
我们已经介绍了在`Swift`中使用`available`属性的所有可能性。您可以运行特定平台和`Swift`版本的代码，现在可以将方法标记为已弃用，过时或重命名。
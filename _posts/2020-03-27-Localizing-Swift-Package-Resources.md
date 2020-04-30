---
layout: post
title: 本地化Swift软件包资源
date: 2020-03-27 12:00:00 +0900
categories: [能工巧匠集, Swift]
tags: [Swift, Xcode, SwiftPackage,本地化]
---


>确保您的Swift软件包为许多语言环境提供了本地化的资源。


## 总览
将Swift软件包中的资源本地化可确保其对尽可能多的开发人员有用。尽早采用本地化资源，以避免以后增加开发成本。

## 声明默认本地化
要本地化包的资源，请将可选参数传递给包清单中的包初始化程序。本示例提供英语作为默认本地化：[defaultLocalization](https://developer.apple.com/documentation/swift_packages/package/3583303-defaultlocalization)

```swift
let package = Package(
    name: "MyLibrary",
    defaultLocalization: "en",
    platforms: [
    ],
    products: [
        // Products define the executables and libraries a package produces, and make them visible to other packages.
    ],
    dependencies: [
        // Dependencies declare other packages that this package depends on.
        // .package(url: /* package url */, from: "1.0.0"),
    ],
    targets: [
        // Targets are the basic building blocks of a package. A target can define a module or a test suite.
        // Targets can depend on other targets in this package, and on products in packages this package depends on.
)
```

在程序包清单中声明的值时，Xcode要求程序包包含本地化资源。`defaultLocalization`

>重要
>
>声明受支持的语言或命名包含本地化资源的目录时，请使用带两个字母的ISO 639-1或带三个字母的ISO 639-2语言代码以及可选的区域或脚本指示符。请参阅以获取更多信息。[CFBundleDevelopmentRegion](https://developer.apple.com/documentation/bundleresources/information_property_list/cfbundledevelopmentregion)

## 将资源添加到特定于语言的目录
要使用Xcode的默认本地化机制，请将本地化的资源放在特定于语言的目录中。特定于语言的目录的名称使用`ISO 639`语言代码和可选的指示符，后跟`.lproj`后缀，并且不包含子目录。例如，在英国使用的英语资源位于名为的目录中。通过将包资源放置在结尾的目录中并使用`ISO 639`语言代码，Xcode可以自动推断语言。`en-GB.lproj.lproj`

将`.lproj`目录放在名为的父目录中，`Resources`这样您就可以识别出它包含软件包资源。

在构建`Swift`软件包时，Xcode会验证软件包的本地化资源并显示警告或错误，以帮助防止运行时出现问题。例如，Xcode检测到：

- .lproj目录中的子目录。

- 缺少语言环境的资源。

- 资源重复，冲突或无法访问。

以下屏幕快照显示了具有本地化资源的Swift软件包的结构。

<img  src="/assets/images/2020/rendered2.png" width="346" height="auto">


Xcode的Project导航器中扩展的Swift软件包的屏幕快照，本地化资源目录位于名为Resources的父目录中。

## 本地化情节提要和界面生成器文件
如果Swift软件包包括情节提要或Interface Builder文件作为资源，请采用基本的国际化方式，以减轻本地化人员直接修改这些文件的需要。要让Xcode自动识别Swift包中的基本本地化，请执行以下操作：

1、在包清单中声明一个值。[defaultLocalization](https://developer.apple.com/documentation/swift_packages/package/3583303-defaultlocalization)

2、例如，Resources,为您的本地化资源创建一个目录。

3、创建一个名为的子目录，并将程序包的故事板和Interface Builder文件放入其中。Base.lproj

4、将.lproj所有受支持语言的Resources目录放置在目录中。

如果您希望显式声明用于基础国际化的资源，请使用流程规则并将[Resource.Localization.base](https://developer.apple.com/documentation/swift_packages/resource/localization/base)传递给它。例如，使用以下命令声明一个支持基本国际化的`.xib`文件：

```swift
.process("path/to/MyViewController.xib", localization: .base)
```

有关基本国际化的更多信息，请参见国际化用户界面。

## 访问本地化资源
Xcode识别.lproj目录中的本地化资源，并自动创建资源束。因此，您可以使用应用开发中可能已经知道的API来访问程序包代码中的本地化资源文件。例如，使用[Foundation](https://developer.apple.com/documentation/foundation)在运行时访问本地化的字符串：

```swift
let localizedString = NSLocalizedString("a_localized_string", bundle: Bundle.module, comment: "a comment")
```

同样，您可以使用来访问本地化的图像资源UIImage：

```swift
UIImage(named: "image name", in: .module, with: nil)
```


有关更多信息，请参见[访问代码中的资源](https://developer.apple.com/documentation/swift_packages/bundling_resources_with_a_swift_package#3578939)。
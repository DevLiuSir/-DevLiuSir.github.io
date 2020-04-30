---
layout: post
title: 使用Xcode创建独立的 SwiftPackage 软件包
date: 2020-03-28 12:00:00 +0900
categories: [能工巧匠集, Swift]
tags: [Swift, Xcode, SwiftPackage]
---


## 使用Xcode创建独立的Swift软件包

> 将可执行或共享代码捆绑到独立的Swift软件包中。


## 总览
Swift软件包是Swift，Objective-C，Objective-C ++，C或C ++代码的可重用组件。他们可以捆绑资源，将其代码出售为二进制文件或依赖其他软件包。使用Swift软件包将可执行代码（例如脚本）捆绑为可执行产品，或创建一个软件包以将可共享代码出售为库产品。出售库产品的软件包有助于提高代码的模块化，使其易于与他人共享代码，并使其他开发人员能够向其应用程序添加功能。


使用Xcode，您可以创建一个新的Swift包，添加代码，资源文件和二进制文件，构建Swift包并运行其单元测试。

![](/assets/images/2020/rendered.png )

流程图显示了Swift软件包的开发生命周期。

## 创建一个Swift包
要创建一个新的Swift包，请打开Xcode并选择File> New> Swift Package。选择一个名称，然后选择一个文件位置。选择“在我的Mac上创建Git存储库”以将软件包置于版本控制下。完成后，Swift包将在Xcode中打开，外观类似于标准Xcode项目。Xcode在创建Swift包时会生成所有必需的文件和文件夹：

- 该文件位于软件包的根级别。它描述了您的Swift软件包的功能。README.md

- 该文件或软件包清单描述了Swift软件包的配置。您可以在Finder中双击它，以Xcode打开包。程序包清单使用Swift和PackageDescription框架定义程序包的名称，产品，目标，对其他程序包的依赖性等。Package.swift

- 源文件位于一个名为的文件夹中，Sources并按进行范围划分[Target](https://developer.apple.com/documentation/swift_packages/target)。Swift包可以包含多个目标，并且按照惯例，每个目标的代码都位于其自己的子文件夹中。

- 单元测试目标位于一个名为的文件夹中Tests，并且遵循与标准目标相同的约定，每个测试目标的代码均位于其自己的子文件夹中。

![屏幕快照](/assets/images/2020/package.png)


## 配置您的Swift软件包
Swift软件包不使用.xcproject或.xcworkspace依赖于其文件夹结构，并使用软件包清单进行其他配置。下面的代码清单显示了一个简单的包清单。它声明MyLibrary目标，并将其作为具有相同名称的库产品出售。

```swift
// swift-tools-version:5.3
import PackageDescription

let package = Package(
    name: "MyLibrary",
    platforms: [
        .macOS(.v10_14), .iOS(.v13), .tvOS(.v13)
    ],
    products: [
        // Products define the executables and libraries a package produces, and make them visible to other packages.
        .library(
            name: "MyLibrary",
            targets: ["MyLibrary", "SomeRemoteBinaryPackage", "SomeLocalBinaryPackage"])
    ],
    dependencies: [
        // Dependencies declare other packages that this package depends on.
    ],
    targets: [
        // Targets are the basic building blocks of a package. A target can define a module or a test suite.
        // Targets can depend on other targets in this package, and on products in packages this package depends on.
        .target(
            name: "MyLibrary",
            exclude: ["instructions.md"],
            resources: [
                .process("text.txt"),
                .process("example.png"),
                .copy("settings.plist")
            ]
        ),
        .binaryTarget(
            name: "SomeRemoteBinaryPackage",
            url: "https://url/to/some/remote/binary/package.zip",
            checksum: "The checksum of the XCFramework inside the ZIP archive."
        ),
        .binaryTarget(
            name: "SomeLocalBinaryPackage",
            path: "path/to/some.xcframework"
        )
        .testTarget(
            name: "MyLibraryTests",
            dependencies: ["MyLibrary"]),
    ]
)
```

软件包清单必须以字符串开头// swift-tools-version:，后跟版本号，例如。// swift-tools-version:5.3

Swift工具版本声明：

- PackageDescription框架的版本

- Swift语言兼容版本来处理清单

- 使用该软件包所需的最低版本的Swift工具

每个版本的Swift都可以对PackageDescription框架进行更新，但是声明了先前Swift工具版本的包可以使用以前的API版本。此行为使您可以利用Swift的新版本，Swift工具和PackageDescription框架，而不必更新软件包清单，也不必失去对现有软件包的访问权限。

要了解有关PackageDescription框架的更多信息，请参见[Package](https://developer.apple.com/documentation/swift_packages/package)。

>注意
>
>当您编辑软件包清单时，Xcode提供代码完成。

## 添加您的代码
按照约定，源文件位于包Sources目录的子文件夹中，该子文件夹与它们所属于的目标名称相同。注意上面的包清单如何声明目标。其源文件位于中，而测试的源文件位于中。您可以使用其他子文件夹来结构化它们。默认情况下，Xcode将所有有效的源文件包含在目标文件夹中。如果您希望显式声明包含的源文件，请在初始化时使用参数传递它们。您还可以将路径传递到目录。`MyLibrarySources/MyLibrarySources/MyLibraryTestssourcesTarget`


![显示了一个独立的Swift软件包，其中包含两个添加的源文件和两个单元测试文件](/assets/images/2020/package.png)


要将源文件添加到Swift包中，请使用您已经知道的工作流程。例如，可以通过将源文件拖到“项目”导航器中，或使用“文件”>“将文件添加到[packageName]”菜单来将源文件添加到包中。目标可以包含Swift，Objective-C / C ++或C / C ++代码，但单个目标不能将Swift与C系列语言混合使用。例如，一个Swift包可以有两个目标，一个目标包含Objective-C，Objective-C ++和C代码，第二个目标包含Swift代码。

## 添加对另一个Swift包的依赖
就像应用程序一样，Swift软件包可以具有软件包依赖项。要声明对远程软件包的依赖关系，请使用将远程软件包的URL作为参数的函数之一。要将本地程序包添加为依赖项，请使用将本地程序包的路径作为参数的函数之一。以下代码片段显示了这两个选项：

```swift
dependencies: [    
    // Dependencies declare other packages that this package depends on.
    .package(url: "https://url/of/another/package.git", from: "1.0.0"),
    .package(path: "path/to/a/local/package/", "1.0.0"..<"2.0.0")],
```


有关声明包依赖关系的所有可能方法，请参见[Package.Dependency](https://developer.apple.com/documentation/swift_packages/package/dependency)。添加依赖项后，您可以将其出售的产品用作[Target.Dependency](https://developer.apple.com/documentation/swift_packages/target/dependency)或使其成为软件包[Product](https://developer.apple.com/documentation/swift_packages/product)的一部分。


## 作为Swift软件包分发二进制文件
您可以选择分发二进制文件，而不是分发出售源文件的Swift软件包。例如，专有的封闭源代码库的创建者经常将它们作为二进制文件提供。请参阅[将二进制框架作为Swift软件包分发](https://developer.apple.com/documentation/swift_packages/distributing_binary_frameworks_as_swift_packages)以了解更多信息。

## 添加软件包资源
在清单文件中声明5.3或更高版本的Swift工具版本，以将资产文件作为包资源添加到Swift包中。例如，Swift包可以包含使用资产目录，情节提要，.strings文件等的用户界面组件。请参阅[使用Swift软件包捆绑资源](https://developer.apple.com/documentation/swift_packages/bundling_resources_with_a_swift_package)以了解更多信息。

## 使您的Swift软件包跨平台兼容
尽管Swift软件包本质上与平台无关，并且包括Linux作为目标平台，但是Swift软件包可以是特定于平台的。使用条件编译块来处理特定于平台的代码并实现跨平台兼容性。以下示例显示了如何使用条件编译块：

```swift
#if os(Linux)

// Code specific to Linux

#elseif os(macOS)

// Code specific to macOS

#endif

#if canImport(UIKit)

// Code specific to platforms where UIKit is available

#endif
```

此外，您可能需要定义最低部署目标。请注意，下面的软件包清单如何通过将最小部署目标作为值传递给初始化程序的`platforms`参数来声明它们[Package](https://developer.apple.com/documentation/swift_packages/package)。但是，将最小部署目标传递给初始化程序并不会将程序包限制为列出的平台。

```swift
// swift-tools-version:5.3
import PackageDescription

let package = Package(
    name: "MyLibrary",
    platforms: [
        .macOS(.v10_14), .iOS(.v13), .tvOS(.v13)
    ],
    products: [
        // Products define the executables and libraries a package produces, and make them visible to other packages.
        .library(
            name: "MyLibrary",
            targets: ["MyLibrary", "SomeRemoteBinaryPackage", "SomeLocalBinaryPackage"])
    ],
    dependencies: [
        // Dependencies declare other packages that this package depends on.
    ],
    targets: [
        // Targets are the basic building blocks of a package. A target can define a module or a test suite.
        // Targets can depend on other targets in this package, and on products in packages this package depends on.
        .target(
            name: "MyLibrary",
            exclude: ["instructions.md"],
            resources: [
                .process("text.txt"),
                .process("example.png"),
                .copy("settings.plist")
            ]
        ),
        .binaryTarget(
            name: "SomeRemoteBinaryPackage",
            url: "https://url/to/some/remote/binary/package.zip",
            checksum: "The checksum of the XCFramework inside the ZIP archive."
        ),
        .binaryTarget(
            name: "SomeLocalBinaryPackage",
            path: "path/to/some.xcframework"
        )
        .testTarget(
            name: "MyLibraryTests",
            dependencies: ["MyLibrary"]),
    ]
)
```

>提示
>
>如果您打算发布不支持所有平台的Swift软件包，请考虑在文件中提及受支持的平台。另外，考虑增加对其他平台的支持以增加其受众。README.md

## 建立目标并进行单元测试
Xcode为包清单中的每个产品创建一个方案。为程序包的构建和运行目标选择一个方案，并在构建应用程序目标时对其进行构建。每个源目标通常至少具有一个相应的测试目标。如果您的软件包包含多个产品，则Xcode会创建一个名称为[packageName] -Package的附加方案来构建所有目标并运行所有单元测试。
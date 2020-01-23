---
layout: post
title:  使用Kingfisher处理网络图片的读取与缓存 
date:   2016-03-08 22:21:49
categories: 能工巧匠集
tags: 能工巧匠集
---


`Kingfisher` 是一个读取网络图片和处理本地缓存的开源库，由 `onevcat` 开发。提到图片缓存库，那么熟悉 `Objective-C` 开发的同学，可能会想起 `SDWebImage`。 没错，`Kingfisher` 除了提供图片的读取和缓存功能，它还是完全使用 `Swift` 实现的。

![](https://raw.githubusercontent.com/onevcat/Kingfisher/master/images/logo.png)



## 安装 Kingfisher

那么，我们就来了解一下 `Kingfisher` 吧, 它支持 `CocoaPods` 和 `Carthage` 包管理，我们可以通过他们来安装。

### `CocoaPods` 的安装方法:

设置 Podfile：

```swift
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '8.0'
use_frameworks!

pod 'Kingfisher'
```

然后运行命令：

```
$ pod install
```

### `Carthage` 的安装方法：

设置 Cartfile:

```
github "onevcat/Kingfisher" >= 1.7
```

然后运行命令：

```
$ carthage update
```

关于 CocoaPods 和 Carthage 的使用，可以参看这两篇内容：

- Carthage 包管理工具，另一种敏捷轻快的 iOS & MAC 开发体验
[](http://swiftcafe.io/2015/10/25/swift-daily-carthage-package)

- 当 Swift 遇上 CocoaPods
[](http://swiftcafe.io/2015/02/10/swift-tips-cocoapods)


当然，如果你不习惯使用包管理，可以进行手动安装，可以直接使用 git 命令将库读取出来：

```
$ git submodule add https://github.com/onevcat/Kingfisher.git
```

### 使用 Kingfisher

`Kingfisher` 对 `UIImageView` 做了扩展。 所以我们只需要引入 `Kingfisher` 库，然后在 UIImageView 的实例上面直接调用即可：

```swift
import Kingfisher

imageView.kf_setImageWithURL(NSURL(string: "http://your_image_url.png")!)
```

### 指定默认图片

因为 Kingfisher 加载图片的时候是异步操作的，所以在图片读取那段时间中，UIImageView 中是没有图片的，所以我们可以设置一个默认图片：

```swift
imageView.kf_setImageWithURL(NSURL(string: "http://your_image_url.png")!, placeholderImage: nil)
```

### 自定义缓存机制

关于缓存，默认情况下，KingFisher 使用图片的 url 作为缓存的 key 值，如果你希望设置自己的缓存 key 值机制，还可以这样：

```swift
let URL = NSURL(string: "http://your_image_url.png")!
let resource = Resource(downloadURL: URL, cacheKey: "your_customized_key")

imageView.kf_setImageWithResource(resource)

```

使用 Resource 类来进行自定义缓存键值的处理。

### 强制刷新

Kingfisher 在默认情况下，每次读取图片的时候，会先判断本地缓存中是否已经存在，如果存在，就使用本地缓存的图片。当然你也可以强制每次都读取网络，只需要指定一个 `Options` 参数即可：

```swift
imageView.kf_setImageWithURL(NSURL(string: "your_image_url")!,
                         placeholderImage: nil,
                              optionsInfo: [.Options(KingfisherOptions.ForceRefresh))
```

Options 参数还有很多应用场景，比如你需要在图片加载出来后，用渐变的方式显示出来：

```swift
imageView.kf_setImageWithURL(NSURL(string: "your_image_url")!,
                         placeholderImage: nil,
                              optionsInfo: [.Transition(ImageTransition.Fade(1)))

```
这里的 Options 参数，就指定了一个1秒钟的渐变显示。

### 回调

当然，还可以在图片加载完成后，指定回调，来进行一些操作：

```swift
imageView.kf_setImageWithURL(NSURL(string: "your_image_url")!,
                         placeholderImage: nil,
                              optionsInfo: nil,
                            progressBlock: { (receivedSize, totalSize) -> () in
                                print("Download Progress: \(receivedSize)/\(totalSize)")
                            },
                        completionHandler: { (image, error, imageURL) -> () in
                            print("Downloaded and set!")
                        }
)
```

### 结语

- 总之，`KingFinsher` 是一个非常方便的开源库，并且它是完全使用 Swift 来创建的，极大的而方便了我们处理图片的工作。
关于 `KingFinsher` 的更多内容，大家可以参看它的 `Github` 主页：

[](https://github.com/onevcat/Kingfisher)



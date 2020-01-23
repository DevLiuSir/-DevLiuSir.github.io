---
layout: post
title:  手把手教你高效快捷的创建Swift Framework
date:   2017-7-29 22:26:49
categories: Swift
tags: Swift
---

## 前言:

 本文是我在工作中开发 Framework 过程中的对踩过的坑走过的弯路的总结,此教程 Swift 和 OC 都适用,文章末尾附可能遇到的问题以及解决方案,希望给各位开发小伙伴一点帮助。

本文主要讲了一下内容:

- 1.动态库和静态库的区别
- 2.如何创建 swift 版的 framework
- 3.如何调试 Framework,两种方法
- 4.关于 Framework 的CPU架构 以及如何合成 framework 的架构让其即支持真机又支持模拟器,两种方法
- 5.在开发 framework 中可能遇到的坑以及解决方案

### 开始：

 在我们开发中最离不开的就是 Framework 比如 UIKit.framework,所以对 framework 应该是比较熟悉的,那么在开发中也经常把自己的所做模块的代码做成 framework,场景如下:

1. 方便给别人使用我们自己的模块
2. 提供给第三方使用,且又不愿意别人看到自己的内部实现逻辑
3. 模块化提高代码的复用性

### 动态库和静态库的区别

#### 静态库：

	 链接时完整地拷贝至可执行文件中，被多次使用就有多份冗余拷贝。

#### 动态库：

	链接时不复制，程序运行时由系统动态加载到内存，供程序调用，系统只加载一次，多个程序共用，节省内存。

#### 区别:
> 静态库和动态库是相对编译期和运行期的：静态库在程序编译时会被链接到目标代码中，程序运行时将不再需要改静态库；而动态库在程序编译时并不会被链接到目标代码中，只是在程序运行时才被载入，因为在程序运行期间还需要动态库的存在。

#### 总结：

> 同一个静态库在不同程序中使用时，每一个程序中都得导入一次，打包时也被打包进去，形成一个程序。而动态库在不同程序中，打包时并没有被打包进去，只在程序运行使用时，才链接载入（如系统的框架如UIKit、Foundation等），所以程序体积会小很多，但是苹果不让使用自己的动态库，否则审核就无法通过。

- 下面就一步一步的演示如何制作一个 framework, 以及制作自己的 framework 中可能会遇到的坑,以及如何解决。作为演示做个比较简单的数组乱序的 framework

##### 新建一个 framework

1. File -> new -> Progect-> 选中 iOS -> Cocoa Touch Framework
![](https://upload-images.jianshu.io/upload_images/2533856-0b7e218bb44be138.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

2. 点击 Next -> Protect Name 为 ArrayDisorderSDK launguage 选为 Swift 点击 Next 新建完成
![](https://upload-images.jianshu.io/upload_images/2533856-d29a76069e73b3e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

3. 在 Framework 中新建文件:

选 source 的时候 可以选择Cocoa Touch Class 或者Swift File 等 都是可以的可以根据自己的需要选择
![](https://upload-images.jianshu.io/upload_images/2533856-eeed4dcd348fd72c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

类名就取为 ArrayDisorder，然后新建完成

![](https://upload-images.jianshu.io/upload_images/2533856-fda708db910a7f51.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

4. 在ArrayDisorder 类中写下数组乱序的代码

```swift
open class ArrayDisorder: NSObject {
    open func disorder (orders:Array<Any>) -> Array<Any> {
        var temp = orders
        var count = Int(temp.count)
        while count > 0 {
            let index = Int(arc4random_uniform(UInt32(Int32(count))))
            let last =  Int(count-1)
            temp.swapAt(index, last)
            count -= 1
        }
        return temp
    }
}
```


- 注意：也许你会发现 在 class 前面或者在disorder方法前有 open 关键字，说到这里先说下和这个地方和oc 制作 Framework 不同的地方
- OC 制作 framework 会生成一个和 framework 名字一样的类 只有.h 文件 内容如下

```objc
//! Project version number for ArrayDisorderSDK.
FOUNDATION_EXPORT double ArrayDisorderSDKVersionNumber;
//! Project version string for ArrayDisorderSDK.
FOUNDATION_EXPORT const unsigned char ArrayDisorderSDKVersionString[];
// In this header, you should import all the public headers of your framework using statements like #import <ArrayDisorderSDK/PublicHeader.h>
上面的这句注释说的很清楚 如果你想让别人外面找到你在 framework 中的类你必须像 <ArrayDisorderSDK/PublicHeader.h> 这种格式一样把 你的类导入这文件中
```

最后一句

```objc
// In this header, you should import all the public headers of your framework using statements like #import <ArrayDisorderSDK/PublicHeader.h>
```

说的很清楚如果你想让别人外面找到你在 framework 中的类你必须像 <ArrayDisorderSDK/PublicHeader.h> 这种格式一样把 你的类导入这文件中,在编译成 SDK 之后生成的头文件中就可以看到你对外开放的类和方法

创建 Swift framework也会生成一个这样的文件但是 则不需要这样导入头文件。只需要在要暴露给使用者的类名和方法名前面写上 Open 或者 Public 当编译成 Framework 之后会生成 "你的 framework 的名字-Swift.h"的文件 在这个文件中你可以看到类名前面 表有 Public 或者 Open 关键字的类.

- 什么时候用 Open 什么时候用 Public?

1. Open 在作用域外是可以被访问,继承 ,用 Open 关键字修饰的开放类成员在作用域之外是可访问和可覆盖的。

2. Pubic 在作用域外是可访问的，但在作用域之外没有子类。公共类成员是可访问的，但在作用域之外是不可覆盖的。

##### 配置你的 Framework

1. 配置动态或者静态库

点击 framework 的 target build settings - linking -> Mach-o Type -> Static Library
或者 Dynamic Library

2. Development Target

在这个 Demo 中 设置为支持 iOS 8
![](https://upload-images.jianshu.io/upload_images/2533856-45e8a3e2960a3737.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

3. 还要配置 运行编译的成的 Build Configuration 为 release 如果不是 release 则在 release 环境下运行会出错
走到这一步,基本 Framework 已经初步完成, Com + B

- **注意:编译的时候 选择的 Device 如果是模拟器 则生成的是 x86架构,仅支持模拟器,在真机上这个编译的 Framework 则不能用 ,如果 Device 不选或者选择你连接在电脑上的真机,则编译成的是arm64 可以在真机上跑但是不能用在模拟器,如果制作的 framework 需要在模拟器和真机上用,则需要把这两个架构合成一块,这个待会下面详细讲**

![](https://upload-images.jianshu.io/upload_images/2533856-14aeac2b494c070d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

点击生成的 Framework 然后 Show In Finder

![](https://upload-images.jianshu.io/upload_images/2533856-7286e0e63e9a98ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

如图中所示,生成了ArrayDisorderSDK.framework 那么这个文件就可以直接给别人使用了.

##### 调试 Framework

在我们写代码时一般都习惯于边写边调试,那么在制作 Framework 时有两种方法调试我们的 Framework

#### 第一种(不推荐)

新建一个名字是 testArrayDisorderDemo的 Progect 如下


![](https://upload-images.jianshu.io/upload_images/2533856-51f76ecdd93862d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)
直接把编译好的 Framework 拖入到 testArrayDisorderDemo中

![](https://upload-images.jianshu.io/upload_images/2533856-cc015989c4caec0a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

然后 import ArrayDisorderSDK

在 ViewDidLoad 中写入如下代码

```swift
  let disOrder = ArrayDisorder() //Framework 中的封装了乱序功能的类
   //disOrder.disorder(orders: [1,2,3,4,5,6,7,8,9]) 调用ArrayDisorder 对外公布的类
   print(disOrder.disorder(orders: [1,2,3,4,5,6,7,8,9]))
   // 输出:[8, 6, 2, 5, 3, 4, 7, 9, 1]
```
到此,我们简单的 framework 就可以算完成了.

但是这中调试framework 的方法效率比较低,我们每一次调试都需要重新把原来的 framework 从testArrayDisorderDemo中删除然后重新导入.这样比较麻烦.那么下面有一种比较方便高效的方法。

#### 第二种 创建一个依赖工程(推荐)

还是testArrayDisorderDemo 这个测试 Framework 的工程

如图所示
![](https://upload-images.jianshu.io/upload_images/2533856-70bf8b01012b89f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

然后把ArrayDisorderSDK 拖入到testArrayDisorderDemo 文件中 如图


![](https://upload-images.jianshu.io/upload_images/2533856-de0d0d60ce15b520.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

然后打开testArrayDisorderDemo

-- > General ->Embedded Binaries-> 点击加号-> add Other -> 选中ArrayDisorderSDK.xcodeproj

如下图
![](https://upload-images.jianshu.io/upload_images/2533856-9091116dec16ad56.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

![](https://upload-images.jianshu.io/upload_images/2533856-8386180030b08b04.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

![](https://upload-images.jianshu.io/upload_images/2533856-8d18da37a680aabf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

再选择 Target 的地方你会发现 现在已经有连个 Taeget 一个是testArrayDisorderDemo 一个是ArrayDisorderSDK

如果选中 testArrayDisorderDemo就是运行 Demo 如果选中ArrayDisorderSDK 就是编译 SDK

这时你会发现可以直接在testArrayDisorderDemo 的 ViewController 中直接 导入ArrayDisorderSDK就可以了 不需要在删除然后再拖入了. 最重要的是 ,当你选中的testArrayDisorderDemo Tagret 运行的时候 当程序走到 framework 中去的时候 你还可以打断点等方式调试. 效率会很高.

> - 注意: 当你修改 framework 中的代码时要想测试下修改的效果,你必需先编译下你的 framework 的 target 如本文中所示的就要先编译下ArrayDisorderSDK的 target,不编译的话就相当于你的 测试你 framework 的 Demo 用的还是原来的 framework 而不是修改后的

#### 关于 Framework 的CPU架构

CPU 的架构在不同的机型上有不同的
主要有一下:

- arm7: 在最老的支持 iOS7的设备上使用
- arm7s: 在 iPhone 5 和 iPhone 5c 上使用
- arm64: 在 iPhone 5s 的64位 ARM 处理器上
- i386: 在32位模拟器上使用
- x86_64: 在64位模拟器上使用

当然一个 Framework 不需要全部支持,可以根据需要.

如上文所说, 编译的时候 选择的 Device 如果是模拟器 则生成的是 x86架构,仅支持模拟器,在真机上这个编译的 Framework 则不能用 ,如果 Device 不选或者选择你连接在电脑上的真机,则编译成的是arm64 和 arm7 可以在真机上跑但是不能用在模拟器,如果制作的 framework 需要在模拟器和真机上用,则需要把这两个架构合成一块,那么这就需要合成架构了.

首先,我们要了解,如何查看一个 framework 的架构

用命令: lipo -info

> 注意 -info 中 - 和 info 没有空格,info 后面有一个空格
如图所示
![](https://upload-images.jianshu.io/upload_images/2533856-9a7ac2bd271944cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

把ArrayDisorderSDK 文件拖入控制台 然后回车
控制台就可以输出ArrayDisorderSDK 的 CPU 架构 如 armv7 arm64
![](https://upload-images.jianshu.io/upload_images/2533856-c316845da0702b2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

![](https://upload-images.jianshu.io/upload_images/2533856-23bde7b4ec8fd9ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

这个是支持真机的,如果支持模拟器则不行的,需要重新编译:

在上图中也能看到 当选中ArrayDisorderSDK.framework show in Finder 时你会发现在上一层有四个文件夹(最多为四个如果你只在 relesae 下真机上编译 就只有一个文件夹) ,主要分为 Debug 和 release 两种环境下的 真机和模拟器
Debug-iphoneos/ Debug-iphonesimulator Debug 下的模拟器
Release-iphoneos/Release-iphonesimulator relesase 下的真机和模拟器

那么我们需要做的就是把Release-iphoneos/Release-iphonesimulator 下的两种架构合成一个

#### 架构合并:

##### 第一种方法(不推荐)

用命令: lipo -create xx $$ -output @@
加入你framework 的名字是 ArrayDisorderSDK

xx: 表示Release-iphoneos 文件夹下的 ArrayDisorderSDK.framework/ArrayDisorderSDK 的路径
$$ : Release-iphonesimulator 文件夹下的 ArrayDisorderSDK.framework/ArrayDisorderSDK

@@: 生成的文件名 是和你的 framework 同名的,比如叫ArrayDisorderSDK

生成的output 出来的ArrayDisorderSDK是需要把Release-iphoneos 文件夹下的 ArrayDisorderSDK.framework 的中的ArrayDisorderSDK 替换掉的.

- 注意:-create 以及-output 在 create 和 output前面以- 是没有空格,但是在create 和 output后面是有一个空格的

上面说的可能不太好懂,那么下面演示一下,化繁为简:
如图
![](https://upload-images.jianshu.io/upload_images/2533856-fab7303fb46a514d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)
总之这种方法还是太麻烦的,容易出错,下面介绍另外一种比较简单的.

#### 第二种:(推荐)

1. 选中ArrayDisorderSDK.xcodeproj 新建一个名字为UniversalArrayDisorder 的 target

![](https://upload-images.jianshu.io/upload_images/2533856-ae1172feccf90214.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

![](https://upload-images.jianshu.io/upload_images/2533856-b74f4d7e92031161.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

![](https://upload-images.jianshu.io/upload_images/2533856-8585a0ec84bd6b31.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

2. 选中 UniversalArrayDisorder --> Build Phases --> 选中加号 --> 选中 New Run Script Phase

![](https://upload-images.jianshu.io/upload_images/2533856-339813d50d16e2a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

![](https://upload-images.jianshu.io/upload_images/2533856-83db04240827c0e5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

然后在 [脚本地址](https://link.jianshu.com/?t=https%3A%2F%2Fgist.github.com%2Fcromandini%2F1a9c4aeab27ca84f5d79)

下载一个名为universal-framework.sh 文件 把文件内容拷贝到如图所示


![](https://upload-images.jianshu.io/upload_images/2533856-b3b684b1bc9739fb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

- 注意:universal-framework.sh 中 ${PROJECT_NAME} 要把这个换成你自己的 framework 的名字

设置完成之后 ,编译UniversalArrayDisorder target 编译成功之后,会自动打开一个生成 Framework 的文件夹,然后再查看架构信息你会发现,即支持真机也支持模拟器啦.

### 你可能会遇到的坑

- 1. 找不到 framework

错误提示:
No such module xxx

#### 解决:

> 原因就是 Framework Search Path 中的路径错了 可以参考 参考地址 解决方案手动设置 这个路径是你向引用你的 framework 的项目拖自定义的 framework 时自动生成的 所以简单的解决方案就是 删除 framework 重新拖入 注意:拖的时候一定要确保 framework 和目标项目在同一个文件夹下,这样就不会出问题了

- 2. 找不到类

错误提示:

```swift
'xxxx' is unavailable: cannot find Swift declaration for this class
 xxxx 表示 framework 中的类名
```

解决:

> framework 的架构错误: 如果如你的 framework 需要在模拟器上跑 那么你的 framework 必须包含x86
如果还需要在真机上跑 那么必须包含arm64 架构 可以用命令检查架构 lipo -info 参考

- 3. 动态库和静态库的问题

错误提示:

```swift
Reason: image not found
 Message from debugger: Terminated due to signal 6
```

解决:

> 动态库 Embedded Binaries没有 添加 your framework name.framework -> 添加
如果是静态库 则不需要添加

如图
![](https://upload-images.jianshu.io/upload_images/2533856-f69b402609b56fa7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

最后:

参考文献:

- [https://medium.com/flawless-app-stories/getting-started-with-reusable-frameworks-for-ios-development-f00d74827d11](https://link.jianshu.com/?t=https%3A%2F%2Fmedium.com%2Fflawless-app-stories%2Fgetting-started-with-reusable-frameworks-for-ios-development-f00d74827d11)

- [https://medium.com/captain-ios-experts/develop-a-swift-framework-1c7fdda27bf1](https://medium.com/captain-ios-experts/develop-a-swift-framework-1c7fdda27bf1)

- [https://www.raywenderlich.com/65964/create-a-framework-for-ios](https://www.raywenderlich.com/65964/create-a-framework-for-ios)

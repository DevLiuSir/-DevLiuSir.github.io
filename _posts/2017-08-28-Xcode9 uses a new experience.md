---
layout: post
title:  Xcode9使用新体验
date:   2017-08-28 22:26:49
categories: Swift
tags: Swift
---


iPhone至今已经十周年了，作为一个重要时间点，今年Apple的WWDC17可谓是憋了大招了。iOS11（ARKit，ML）、Swift4、Xcode9。Xcode9相较之前的版本还是有很多实打实的新功能的，目前AppStore已经推送最新版，为此本人写一下个人体验。

## 头文件跳转

头文件跳转发生了改变，Xcode9之前点击类名直接跳转到类头文件里面，现在成了actions了，


![Xcode9_2.png](http://upload-images.jianshu.io/upload_images/224953-38c150c82059d67d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![xcode9_2.1.png](http://upload-images.jianshu.io/upload_images/224953-fc6d7b0b1661c92a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- Jump to Definition(^⌘)：跳转类头文件或定义
- Show Quick Help(⌥)：显示帮助文档
- Edit All in Scope：编辑文档内所有匹配内容
- Extract Function: 抽取成函数
- Extract Method: 抽取成方法，至于和上面功能有啥区别，赶快试一下会让你喜欢上这两个功能的
- Actions: 提供快速搜索三个功能，也许Apple认为这样改变确实不方便了，毕竟和我们预想页面中间多了一步。

![Xcode9_3.png](http://upload-images.jianshu.io/upload_images/224953-0944889a79698fa4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

不过有些小伙伴说了，我虽然喜欢这个功能，但是我CMD+左击 90%的情况是跳转头文件，这样每次需要选择岂不是很麻烦，哈哈 别着急 有办法
路径是Xcode Preference -> Navigation -> Command-click on Code选项改成Jumps to Definition


![xcode9_2.2.png](http://upload-images.jianshu.io/upload_images/224953-fa3206761658f325.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 错误

在Xcode9之前， 错误信息只会在错误处后面一行显示，但是错误信息过多的时候，在看错误信息的时候难免有些不方便，现在好了，当点击错误信息前面的红点时，错误信息会完全展开，并且有fix功能。


![Xcode9_4.png](http://upload-images.jianshu.io/upload_images/224953-b26308d98fa1dc61.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Xcode9_5.png](http://upload-images.jianshu.io/upload_images/224953-75b3b1c0a0e59d31.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 代码块范围

当将光标放在class、func、if、for、{}、()等关键字时，按住command键，Xcode9将提示对应的代码块范围。当点击鼠标时好会出现对应的actions。例如：


![Xcode9_6.png](http://upload-images.jianshu.io/upload_images/224953-9839d0880d46b353.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击 `Add "else" Statement`或者`Add "else if" Statementaction`的时候，Xcode会自动给我们增加else分支内容


![Xcode9_7.png](http://upload-images.jianshu.io/upload_images/224953-1dcda99144387a51.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

厉害的来了，当点击`Extract Method`的时候， Xcode会将分支自动抽取成func， 手动对此功能点赞👍呀！这以后抽取代码块的时候太方便了。

![Xcode9_8.png](http://upload-images.jianshu.io/upload_images/224953-0cab8b2774a67ba6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## New Group新变化

在Xcode9之前,我们在工程文件下New Group的时候,Xcode只会创建虚拟文件目录(这点很痛苦), 还得自己手动创建本地实体文件目录并关联之. Xcode9之后New Group将会同步创建本地实体文件目录. 如果还想像之前那样只是建立虚拟的group,需要选择New Group without Folder即可

## Asset Catalogs

现在Assets.xcassets不光能增加图片，也能增加有名字的颜色


![Xcode9_9.png](http://upload-images.jianshu.io/upload_images/224953-f6ed454c37da34ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Xcode9_10.png](http://upload-images.jianshu.io/upload_images/224953-fd7a624d62bfeb3c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用的时候类似引用图片方式：

```swift
view.backgroundColor = UIColor(named: "color")
```

> 以后管理全局颜色就和管理图片一样一样的啦 方便极了

## Source Control

Xcode9对源代码管理增加了很大的支持，在Preferences中的Source Control增加了Git和Subversion。并且在左侧的Navigator中可以很清楚明了的查看提交信息


![image.png](http://upload-images.jianshu.io/upload_images/224953-b89c4690d1817d41.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在左侧的Navigator中，有Branches、Tags、Remotes，这是相当于集成了SourceTree集成进去了呀

在Accounts中可以增加GitHub账户


![image.png](http://upload-images.jianshu.io/upload_images/224953-20d4da4a974a8af9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

而且GitHub账户信息展示的也很齐全


![image.png](http://upload-images.jianshu.io/upload_images/224953-c3e1a5f2122007b3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

更多的关于Source Control操作不在一一赘述，大家可以自己研究一下

## 兼容性

Xcode9之后支持了Swift4.0和Swift3.2，默认使用的是Swift4.0。你可以再target的Build Settings的Swift Language Version中选择Swift版本！

想当年做Swift2到Swift3的转换时，真是满满的泪呀！


![image.png](http://upload-images.jianshu.io/upload_images/224953-56265575848d7997.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 调试应用

### Simulator

Xcode9这次对Simulator进行了非常大的升级，特别是外观发生了变化

最新的模拟器拟物化了，增加了Home键 左上的静音和声音键，右上的关机键都可以使用

模拟器大小也可以随意调整大小，不像之前只有cmd+1、2、3、4的尺寸，现在将光标放在模拟器四角之一上就可以随意调整模拟器大小了

现在可以运行多个模拟器，方便我们测试不同设备情况下页面的情况，


![image.png](http://upload-images.jianshu.io/upload_images/224953-dcbedcf3c157f81f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/224953-8d4f21f28b53664d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 真机调试

之前如若要真机调试必须要连接数据线，现在方便了，可以通过WIFI无线安装App了，
不过需要Xcode9并且运行iOS11+的版本才能使用，
还有就是Mac和iPhone必须连接同一个WiFi网络。
操作如下：
1.用数据线连接Mac和iPhone
2.选择Xcode menu的Window -> Devices and Simulator


![image.png](http://upload-images.jianshu.io/upload_images/224953-00a78607a13fc8c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.勾选Connect via network如果通过网络成功连接到iPhone，就可以拔掉数据线愉快的进行无线真机调试
同时也可以通过输入iPhone的IP进行连接。



![image.png](http://upload-images.jianshu.io/upload_images/224953-786d214f34c271f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 语言特性

之前判断iOS版本需要获取Device Version：[[UIDevice currentDevice] systemVersion] 现在判断可以像Swift方式那样简单了

```swift
    if (@available(iOS 11.0, *)) {
        // iOS 11 Later
    } else {
        
    }
```
    
## 新的编译系统

Apple提供了一套新的用纯Swift编写的编译系统,据说对编译速度的提升帮助很大,目前并没有成为默认.如果想要体验,需要手动设置一下, File -> Project Settings..即可更改


![xcode9_11.png](http://upload-images.jianshu.io/upload_images/224953-7291c89f24243070.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 其他

按住command + +或-，可以将代码放大或缩小， 这以后演示代码就方便了不少

## 总结

关于Xcode9就简单介绍这里，还有很多其他的新特性。作为十周年之际，Apple也是拿出了很大的诚意，更多的功能可在Xcode的Help-> Xcode Help中查找

## 参考

[What's New in Xcode9](https://developer.apple.com/library/content/documentation/DeveloperTools/Conceptual/WhatsNewXcode/xcode_9/xcode_9.html)
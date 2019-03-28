---
layout: post
title: CocoaPods安装使用心得
date: 2015-03-08 22:21:49 +0900
categories: [能工巧匠集, CocoaPods]
tags: [CocoaPods, iOS, Ruby]
---

## 前言

- CocoaPods是一个负责管理iOS项目中第三方开源代码的工具。
- 安装

由于网上的教程基本都大同小异，但细节之处还不是很完善，所以借机会在这里补充下：
注：要使用CocoaPods，那就要下载安装它，而下载安装CocoaPods需要Ruby环境</p>

### 1.Ruby环境搭建

当前安装环境为Mac Pro 10.11.1。Mac  OS本身自带Ruby，但还是更新一下保险，因为升级了系统之后,可能会出现一些意想不到的情况,最好还是更新一下Ruby.


- 查看下当前ruby版本：打开终端输入 ruby -v（确实安装了，不过iOS9使用https协议替换http协议,所以以前挂靠在淘宝下ruby的源的路径也要修改下，顺便更新下ruby）</p><p>   


```swift
CYdediannao:~ lcy$  ruby -v
ruby 2.0.0p645 (2015-04-13 revision 50299) [universal.x86_64-darwin15]
```


- 淘宝已经停止基于 HTTP 协议的镜像服务, 请在配置中使用 HTTPS 协议代替


```swift
CYdediannao:~ lcy$ gem sources - l
*** CURRENT SOURCES ***
http://ruby.taobao.org/
CYdediannao:~ lcy$ gem sources --add https://ruby.taobao.org/ --remove http://ruby.taobao.org/

https://ruby.taobao.org/ added to sources
http://ruby.taobao.org/ removed from sources
```


- gem sources -l  （用来检查使用替换镜像位置成功，然后升级ruby


```swift
CYdediannao:~ lcy$ sudo gem install rails
```


```swift
或者是 <code>CYdediannao:~ lcy$ sudo gem update --system
```

### 2.下载安装CocoaPods


```swift
CYdediannao:~ lcy$ sudo gem install cocoapods
```

### 出现的错误###：原因是该文件夹没有修改的权限

```swift
CYdediannao:~ lcy$ sudo gem install cocoapods 
Fetching: nap-1.0.0.gem (100%)
Successfully installed nap-1.0.0
Fetching: molinillo-0.4.0.gem (100%)
Successfully installed molinillo-0.4.0
Fetching: cocoapods-trunk-0.6.4.gem (100%)
Successfully installed cocoapods-trunk-0.6.4
Fetching: cocoapods-try-0.5.1.gem (100%)
Successfully installed cocoapods-try-0.5.1
Fetching: cocoapods-stats-0.6.2.gem (100%)
Successfully installed cocoapods-stats-0.6.2
Fetching: cocoapods-search-0.1.0.gem (100%)
Successfully installed cocoapods-search-0.1.0
Fetching: cocoapods-downloader-0.9.3.gem (100%)
Successfully installed cocoapods-downloader-0.9.3
Fetching: xcodeproj-0.28.2.gem (100%)
ERROR:  While executing gem ... (Errno::EPERM)
    Operation not permitted - /usr/bin/xcodeproj
```


10.11以上系统使用命令： sudo gem install -n /usr/local/bin cocoapods</h3>
    

```swift
CYdediannao:~ lcy$ sudo gem install -n /usr/local/bin cocoapods
Password:
Successfully installed xcodeproj-0.28.2
Fetching: cocoapods-core-0.39.0.gem (100%)
Successfully installed cocoapods-core-0.39.0
Fetching: cocoapods-0.39.0.gem (100%)
Successfully installed cocoapods-0.39.0
Parsing documentation for xcodeproj-0.28.2
Installing ri documentation for xcodeproj-0.28.2
Parsing documentation for cocoapods-core-0.39.0
Installing ri documentation for cocoapods-core-0.39.0
Parsing documentation for cocoapods-0.39.0
Installing ri documentation for cocoapods-0.39.0
3 gems installed
```


### 3.使用命令 pod search AFNetworking 查找某一个库，看cocoapods有没有安装好，搜索结果如下，已经安装好了cocoa pods</h4><p>

```swift
-> AFNetworking (2.6.0)
   A delightful iOS and OS X networking framework.
   pod 'AFNetworking', '~> 2.6.0'
   - Homepage: https://github.com/AFNetworking/AFNetworking
   - Source:   https://github.com/AFNetworking/AFNetworking.git
   - Versions: 2.6.0, 2.5.4, 2.5.3, 2.5.2, 2.5.1, 2.5.0, 2.4.1, 2.4.0, 2.3.1,
   2.3.0, 2.2.4, 2.2.3, 2.2.2, 2.2.1, 2.2.0, 2.1.0, 2.0.3, 2.0.2, 2.0.1, 2.0.0,
   2.0.0-RC3, 2.0.0-RC2, 2.0.0-RC1, 1.3.4, 1.3.3, 1.3.2, 1.3.1, 1.3.0, 1.2.1,
   1.2.0, 1.1.0, 1.0.1, 1.0, 1.0RC3, 1.0RC2, 1.0RC1, 0.10.1, 0.10.0, 0.9.2,
   0.9.1, 0.9.0, 0.7.0, 0.5.1 [master repo] - 2.6.0, 2.5.4, 2.5.3, 2.5.2, 2.5.1,
   2.5.0, 2.4.1, 2.4.0, 2.3.1, 2.3.0, 2.2.4, 2.2.3, 2.2.2, 2.2.1, 2.2.0, 2.1.0,
   2.0.3, 2.0.2, 2.0.1, 2.0.0, 2.0.0-RC3, 2.0.0-RC2, 2.0.0-RC1, 1.3.4, 1.3.3,
   1.3.2, 1.3.1, 1.3.0, 1.2.1, 1.2.0, 1.1.0, 1.0.1, 1.0, 1.0RC3, 1.0RC2, 1.0RC1,
   0.10.1, 0.10.0, 0.9.2, 0.9.1, 0.9.0, 0.7.0, 0.5.1 [master-1 repo]
   - Subspecs:
     - AFNetworking/Serialization (2.6.0)
     - AFNetworking/Security (2.6.0)
     - AFNetworking/Reachability (2.6.0)
     - AFNetworking/NSURLConnection (2.6.0)
     - AFNetworking/NSURLSession (2.6.0)
     - AFNetworking/UIKit (2.6.0)


-> AFNetworking+AutoRetry (0.0.5)
   Auto Retries for AFNetworking requests
   pod 'AFNetworking+AutoRetry', '~> 0.0.5'
   - Homepage: https://github.com/shaioz/AFNetworking-AutoRetry
   - Source:   https://github.com/shaioz/AFNetworking-AutoRetry.git
   - Versions: 0.0.5, 0.0.4, 0.0.3, 0.0.2, 0.0.1 [master repo] - 0.0.5, 0.0.4,
   0.0.3, 0.0.2, 0.0.1 [master-1 repo]


-> AFNetworking+Ext (1.2.1)
   AFNetworking的封装, 并提供一个 UIImageView+DYLoading  cache in fileSystem+memory
   pod 'AFNetworking+Ext', '~> 1.2.1'
   - Homepage: https://github.com/junhaiyang/AFNetworkingExt
   - Source:   https://github.com/junhaiyang/AFNetworkingExt.git
   - Versions: 1.2.1, 1.2, 1.1, 1.0, 0.5, 0.4, 0.3 [master repo] - 1.2.1, 1.2,
   1.1, 1.0, 0.5, 0.4, 0.3 [master-1 repo]
   - Subspecs:
     - AFNetworking+Ext/Base (1.2.1)
     - AFNetworking+Ext/AFCustomRequestOperation (1.2.1)
     - AFNetworking+Ext/AFDownloadRequestOperation (1.2.1)
     - AFNetworking+Ext/AFTextResponseSerializer (1.2.1)
     - AFNetworking+Ext/example (1.2.1)
     - AFNetworking+Ext/UIKit (1.2.1)
     - AFNetworking+Ext/UIKit/UIImageView+DYLoading (1.2.1)
```


#### 还有很大篇幅的结果，此处省略。。。


### 4.cocoapods的使用</h2>


- 新建一个项目，名字podsTest项目
![](http://upload-images.jianshu.io/upload_images/830888-25c7ce2edc1537e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


打开项目所在的文件夹，在终端敲入 cd 将文件夹拖拽到终端

或者用cd打开项目所在文件夹（注意：包含podsTest文件夹、podsTest.xcodeproj、podsTestTest、podsTestUITests的那个总目录）如图：

![](http://upload-images.jianshu.io/upload_images/830888-135856a82cc057d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/830888-bc6bbedf65e2da2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



- 建立Podfile（配置文件）


> 1.接着上一步，终端输入 vim Podfile
> ![](http://upload-images.jianshu.io/upload_images/830888-ff9367af4eea800b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 2.键盘输入 i，进入编辑模式
> ![](http://upload-images.jianshu.io/upload_images/830888-82d38c7fcfc4d8c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 3.内容按这个格式输入
> ```swift
  platform :ios, '7.0'
　pod 'MBProgressHUD', '~> 0.9.1'
　pod 'ASIHTTPRequest', '~> 1.8.2'
　pod 'SDWebImage', '~> 3.7.3'
　```

- 注意：　如果你不知道这些框架目前的版本是，可以使用命令pod search +框架名称查看 相应的信息
![](http://upload-images.jianshu.io/upload_images/830888-31adb37f17d235b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
　

> 4.然后按Esc，并且输入 shift +“ ：”号进入vim命令模式，然后在冒号后边输入wq 按回车键，保存并且退出。

 
 > 5.结果如下图：
![](http://upload-images.jianshu.io/upload_images/830888-1f1a82c28b6110c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 6.发现podTest项目总目录中多一个Podfile文件

> ![](http://upload-images.jianshu.io/upload_images/830888-7aadbe1cfd34ad7e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> 7.激动人心的时刻到了：确定终端cd到项目总目录，然后输入 pod install，等待一会，框架安装好了如图所示，多出了3个文件夹/文件夹。
![](http://upload-images.jianshu.io/upload_images/830888-08526c1690c9f975.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 注意：现在打开项目不是点击 podTest.xodeproj了，而是点击 podsTest.xcworkspace

![](http://upload-images.jianshu.io/upload_images/830888-374b5624ae6ecf9c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

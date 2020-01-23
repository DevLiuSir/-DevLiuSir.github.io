---
layout: post
title:   发布开源项目到cocoapods
date:   2018-1-2 22:26:49
categories: Swift
tags: Swift
---

![](https://upload-images.jianshu.io/upload_images/67565-84f2b56fc5f84441.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

### 一、前言

这两天被cocoapods折磨的心力憔悴。看cocoapods官网的添加支持，但是介绍的（ying）比(yu)较(tai)简(cha)单，而且有的步骤也没有写上，导致看着官方文档也没有成功，后来查阅了简书、CocoaChina等等，还是已经接近崩溃。没有一个完整的介绍。索性多个文档对比测试，最后终于成功的让自己Github上的库成功支持Cocoapods安装。

### 二、开始

这里我将从最初的开始进行介绍，包括Github上创建项目已经上传项目，到最后的支持Cocoapods。
步骤如下：

代码上传Github
创建podspec文件，并验证是否通过
在Github上创建release版本
注册CocoaPods账号
上传代码到CocoaPods
检查上传是否成功
#### 1.代码上传Github

首先我们打开github.com，然后创建自己的项目工程：

![](https://upload-images.jianshu.io/upload_images/67565-5710f6a33a663094.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


这里注意那个MIT License，在后面添加Cocoapods支持的时候会用到（稍后介绍）。然后点击创建即可。
然后用SouceTree将代码down到本地，将自己的项目放到里面，文件夹如图所示：
![](https://upload-images.jianshu.io/upload_images/67565-51958b6478a21024.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/206)


这里的LICENSE就是刚才说的MIT License添加的文件。RSADemo是示例工程，ZGRSAEncryptor就是提供给他人使用的库。（ZGEncrptor(For java).zip是后端的代码）

然后提交到Github就可以了。

#### 2.创建podspec文件

我们使用终端到工程目录下：
![](https://upload-images.jianshu.io/upload_images/67565-befbc4492842382a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


然后执行下面的命令：

```swift
pod spec create ZGRSAEncryptor
```

这里的`ZGRSAEncryptor`就是`pod`添加市的名字（例如`MBProgressHUD`）。执行完后的结果：
![](https://upload-images.jianshu.io/upload_images/67565-c41091cdc7b3f8cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


此时在工程文件夹下也会多一个 `ZGRSAEncryptor.podspec`文件。这里我用Sublime Text打开并做了如下编辑：

```swift
Pod::Spec.new do |s|
  s.name         = "ZGInfoCollection"
  s.version      = "1.0.0"
  s.summary      = "A library for collection device info."
  s.homepage     = "https://github.com/ScottZg/ZGInfoCollection"
  s.license      = "MIT"
  s.author             = { "ScottZg" => "scottzg@126.com" }
  s.source       = { :git =>    "https://github.com/ScottZg/ZGInfoCollection.git", :tag => "#{s.version}" }
  s.requires_arc = true
  s.ios.deployment_target = "7.0"
  s.source_files  = "ZGInfoCollection/*.{h,m}"
end
```

- name: 类库的名称
- version: 库的版本
- summary: 介绍语
![](https://upload-images.jianshu.io/upload_images/67565-9b35beb1b4d7727a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


- homtepage: Github上项目地址
- license: 许可证
- author: 作者 
- source: 项目的https链接地址
- source_files: 要共享的代码，这里是ZGRSAEncryptor下面的所有代码。
接下来执行下面的命令进行验证：

```swift
pod lib lint ZGRSAEncryptor.podspec
```

结果多种多样，如果有错，则按照提示进行改错即可。在这里，我执行的结果如下图：
![](https://upload-images.jianshu.io/upload_images/67565-8b3f2248520c9d8f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


发现了多个警告，只要不是错误就行，警告可以直接忽略（红色也提示如何忽略）：

```swift
pod lib lint ZGRSAEncryptor.podspec --allow-warnings
```

结果如下：
![](https://upload-images.jianshu.io/upload_images/67565-56b08bd1761a6e53.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


当看到 ` ZGRSAEncryptor passed validation `之后，就说明验证通过了。

#### 3 在Github上创建release版本

打开项目的目录，然后创建release版本的类库：
![](https://upload-images.jianshu.io/upload_images/67565-eff97ac2b3929182.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


点击 箭头指向开始创建`release`版本，（点击` Create a new release `）：
![](https://upload-images.jianshu.io/upload_images/67565-9d5cb60e06e907dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


点击 `Publish release` 即可。创建完成后如图所示：
![](https://upload-images.jianshu.io/upload_images/67565-a990a938aafe383b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


这样第三步就完成了

#### 4 注册CocoaPods账号

执行命令行：

```swift
pod trunk register 邮箱地址 ‘用户名’ —description='描述信息'
```

执行完之后结果如下：
![](https://upload-images.jianshu.io/upload_images/67565-ac3275ae56d39c94.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


黄色提示已经发送了一个验证码到邮箱，你可以打开你的邮箱验证即可。打开邮件中的链接后如下：
![](https://upload-images.jianshu.io/upload_images/67565-fda837973deeebba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


这样就成功注册了Cocoapods账号。
可以用

```swift
pod trunk me
```

检查是否创建成功。成功的结果如下：
![](https://upload-images.jianshu.io/upload_images/67565-127cfd39a8e4c317.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


#### 5 上传代码到CocoaPods

首先检测文件格式的有效性：

```swift
pod spec lint
```

结果如下：
![](https://upload-images.jianshu.io/upload_images/67565-931990fc7e7827e5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


没有错误，但是有警告。可以使用 --allow-warnings忽略：
![](https://upload-images.jianshu.io/upload_images/67565-36b6fa3b9416e77a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


出现`passed validation`就说明通过验证了。然后执行：

```swift
pod trunk push ZGRSAEncryptor.podspec  --allow-warnings
```

执行结果如下：（速度应该有的慢）
![](https://upload-images.jianshu.io/upload_images/67565-3c954ea799cde829.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


说明了已经上传成功。

#### 6 检查上传是否成功

使用

```swift
pod search ZGRSAEncryptor
```

结果如下：
![](https://upload-images.jianshu.io/upload_images/67565-8b0828e9c06bb1de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

ok，已经成功了。这样就可以让其他人进行搜索使用了。

#### Cocoapods: pod search无法搜索到类库的解决办法:

##### 执行pod setup

- 在终端输入`pod setup` ,会出现 `Setting up CocoaPods master repo` ，等几分钟，会输入`Setup completed`，说明 `pod setup` 执行成功。
- 结果 `pod search` 还是失败
- 在终端输入`pod search RxSwift`
- 依然还是提示 `Unable to find a pod with name, author, summary, or descriptionmatching 'RxSwift'。`
- 但是我输入 `pod search pop，`却有相应的结果。

##### 删除 ~/Library/Caches/CocoaPods目录下的search_index.json 文件

- `pod setup`成功后会生成`~/Library/Caches/CocoaPods/search_index.json`文件。
- 终端输入 `rm ~/Library/Caches/CocoaPods/search_index.json`
- 删除成功后再执行`pod search`

##### 执行 pod search

- 终端输入：`pod search RxSwift` (不区分大小写)
- 输出：`Creating search index for spec repo 'master'.. Done!`，稍等片刻就会出现所有带 `RxSwift` 字段的类库出现。

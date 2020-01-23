---
layout: post
title:  iOS -制作Bundle资源文件包以及正确使用
date:   2018-01-21 22:21:49
categories: 能工巧匠集
tags: 能工巧匠集
---


### 一、创建Bundle项目

![](http://ww1.sinaimg.cn/large/006tNc79ly1g3mtlkwazdj314k0t8diq.jpg)


### 二、在bundle资源包中添加图片
方式一：使用Asset管理图片

在Xcode自带的Assets里面，可以自动识别图片是二倍屏还是三倍屏，图片名称以@2x，@3x为后缀，拖到Assets文件里会自动识别位置

1>创建Asset文件

![](http://ww1.sinaimg.cn/large/006tNc79ly1g3mtlkq71dj314k0t877j.jpg)

2>拖入对应的图片

![](http://ww1.sinaimg.cn/large/006tNc79ly1g3mtlkjfmjj31fs0b40ut.jpg)

方式二：

创建images文件夹，如图所示，再拖入对应后缀名的图片

![](http://ww4.sinaimg.cn/large/006tNc79ly1g3mtn7rta9j30yu04ugmh.jpg)

### 三、生成Bundle包
Command + B后生成Bundle包，点击Products里面的bundle文件

![](http://ww3.sinaimg.cn/large/006tNc79ly1g3mtn7nv4vj30ls0h0dhv.jpg)

### 四、集成到项目中
将Bundle资源包放到项目的任意（或指定）的文件夹下

![](http://ww2.sinaimg.cn/large/006tNc79ly1g3mtn7kf33j30nc0eg0v7.jpg)


### 五、代码调用Bundle包的图片资源（写在UIImage的extension里）：
##### 1>方式一加载图片（使用Asset管理图片时），使用以下方法：


```swift
    /// 从Bundle中加载图片
    ///
    /// - Parameter filename: 图片名称
    /// - Returns: 返回UIImage对象
class func bundleImageNamed(_ filename: String) -> UIImage? {
	let path = "\(Bundle.main.resourcePath ?? "YourBundleName.bundle")"
	return UIImage(named: filename, in: Bundle(path: path), compatibleWith: nil)
}
```


##### 2>方式二加载图片

生成的资源包右键查看包内容，可以看到图片的路径，如图所示

![](http://ww3.sinaimg.cn/large/006tNc79ly1g3mtn7f2jjj30wc0bc76j.jpg)

代码调用如下


```swift
/// 从Bundle中加载图片	
///
/// - Parameter filename: 图片名称
/// - Returns: 返回UIImage对象
class func bundleImageNamed(_ filename: String) -> UIImage? {
	let pathName = "YourBundleName.bundle/Contents/Resources/\(filename)"
	let url = "\(Bundle.main.resourcePath ?? "")/\(pathName)"
	return UIImage(contentsOfFile: url )
}
```

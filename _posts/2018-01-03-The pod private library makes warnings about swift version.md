---
layout: post
title:  pod私有库制作关于swift version的警告
date:   2018-01-03 22:21:49
categories: 能工巧匠集
tags: 能工巧匠集
---


在制作pod私有库时，验证podspec过程中经常会遇到 这个警告：

![](http://ww3.sinaimg.cn/large/006tNc79ly1g3n5yie1gij315o0u0x6g.jpg)

- WARN  | [iOS] swift: The validator used Swift 4.2 by default because no Swift version was specified. To specify a Swift version during validation, add the `swift_version` attribute in your podspec. Note that usage of the `--swift-version` parameter or a `.swift-version` file is now deprecated.

很多人因为警告可以使用 `--allow-warnings` 命令忽略，所以不关心验证过程中的警告。

不过其实这个警告的解决方案很简单，网上也有很多教程，其实仔细阅读这条警告的解释：
大约的意思是，你没有指定使用的swift版本，验证器默认使用的是`swift4.2`，可能会出现问题，希望你指定swift版本

##### 所以很简单的解决方案在.podspec 配置文件中添加（PS: 这里的4.0是根据你使用的版本来指定的，不一定是4.0，也许几年后这里可能就是5.0了）

<font color = red size = 2> s.swift_version = '4.0' </font> 

### 其他解决方案：

##### 1.在终端里到你工程目录下执行这条命令 ：
 
<font color = red size = 2> echo "4.0" > .swift-version </font>

##### 2.在验证配置文件的时候指定swift版本：

<font color = red size = 2> pod spec lint prcject.podspec --swift-version=4.0 –verbose
 </font> 


> 请注意： `s.swift_version = '4.0'` 和 `echo "4.0" > .swift-version`，这两个解决方案不要同时做. 制作pod私有库时，出现这个错误原因主要是 在podspec文件中指定了 s.swift_version，并且在本地还创建了.swift-version这个文件，这两处指定版本不同就会出现问题。要么删掉.swift-version文件，要么.podspec文件中不指定版本.


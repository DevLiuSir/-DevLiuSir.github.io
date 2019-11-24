---
layout: post
title:  iOS获取App ipa包以及资源文件
date:   2018-02-18 22:21:49
categories: 能工巧匠集
tags: 能工巧匠集
---

## 要获得线上APP的ipa文件，现在有以下几种方案

- 1.通过PP助手下载安装到手机的应用
- 2.通过iTools助手下载安装到手机的应用
- 3.通过`Apple Configurator 2`（Mac商店）获取


## 前两种方案网上的教程很多，这里只介绍第三种方案

首先 去Mac上的App Store下载`Apple Configurator 2`

然后把iphone连接上Mac，点击`Apple Configurator 2` 菜单中->账户->登陆（用连接设备的Apple ID）

![](/assets/images/2018/ipa-Resource-1.jpg)

登录Apple ID

![](/assets/images/2018/ipa-Resource-2.jpg)

备份iPhone的内容（避免数据丢失，非必选）

所有设备->选中当前iPhone->添加->应用，找到您想要ipa的那个应用->添加

![](/assets/images/2018/ipa-Resource-3.jpg)

![](/assets/images/2018/ipa-Resource-4.jpg)

下载ipa包中

![](/assets/images/2018/ipa-Resource-5.jpg)

因为你手机中已经存在了当前应用，所以会提示，该应用已经存在， 是否需要替换？

此时，不要点任何按钮！不要点任何按钮！不要点任何按钮！

![](/assets/images/2018/ipa-Resource-6.jpg)

不要操作`Apple Configurator 2`，让它保持上图的状态，然后打开Finder前往文件夹，或者直接快捷键`command+shift+G`

并输入下面路径

```
~/Library/Group Containers/K36BKF7T3D.group.com.apple.configurator/Library/Caches/Assets/TemporaryItems/MobileApps/
```


![](/assets/images/2018/ipa-Resource-7.jpg)

![](/assets/images/2018/ipa-Resource-8.jpg)


点击前往，打开ipa包所在文件。将ipa文件copy出来。

![](/assets/images/2018/ipa-Resource-9.jpg)

这时候别忘了点击Apple Configurator 2窗口中的停止，你会发现刚才目录下的文件也消失了

拿到ipa文件后，你可以将后缀.ipa改为.zip,然后解压

![](/assets/images/2018/ipa-Resource-10.jpg)

就可以看到Payload下的包，显示包内容可以看到部分APP的资源以及Assets.car。

![](/assets/images/2018/ipa-Resource-11.jpg)

如果你要解压Assets.car， 可以使用github上的工具 [https://github.com/pcjbird/AssetsExtractor](https://github.com/pcjbird/AssetsExtractor)
最后，提取出来的资源文件，大家要注意版权，仅供参考，不要直接拿来商业使用。
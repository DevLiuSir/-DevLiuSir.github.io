---
layout: post
title:  使用 Workflow＋Charles下载任意版本的App
date:   2017-06-08 22:22:49
categories: 逆向
tags: 逆向
---

自从去年设备上 iOS 9.3.5 越狱出了问题之后，我又回到了没有越狱的状态。于是就半路出家又去少数派学习了一年的 App 玩法，学有所成就回来分享一个可以用来给逆向工程提供便利的小技巧：使用 Workflow ＋ Charles 下载任意版本的 App。

## 首先我们要准备好以下的软件：
1. [Charles](https://www.charlesproxy.com/)
2. [Workflow](https://itunes.apple.com/cn/app/workflow-powerful-automation-made-simple/id915249334?l=en&mt=8) （ iOS ）
3. [iMazing](https://imazing.com/)

### 在 Mac／Win 上安装好 Charles,并安装 SSL CA 证书。

![](http://7xibfi.com1.z0.glb.clouddn.com/uploads/default/optimized/2X/6/6328c80f0eb2273c186d487c713b73c565c4abbf_1_690x244.png)

### 在钥匙串中将证书设置为始终信任。

![](http://7xibfi.com1.z0.glb.clouddn.com/uploads/default/optimized/2X/4/49e0de34bced201058af579018875d245da68050_1_690x421.png)



使用场景举例：我目前手里可越狱的设备还有一台 iPhone 4 ，最近看了 [iOS Hacking Guide](https://web.securityinnovation.com/hubfs/iOS%20Hacking%20Guide.pdf) 里面第 93 页提到的了使用 Cycript 调试，不用输入密码即可直接进入 App 界面的方法引起了我的兴趣。我自己目前在用 1Password 来管理我的所有密码，所以我就想测试是否能够用 Cycript 绕过 1Password 的密码验证界面呢？但是很不幸的是我尝试去 App Store 下载 1Password，它提示最低需要 iOS 8兼容。使用 App Admin ?也是最低需要 iOS 8 才可以安装。所以还是自己动手丰衣足食下载一个兼容 iOS 7 的 1Password 来测试吧。

### 在 iOS 设备上安装这个Workflow规则：[版本信息](https://workflow.is/workflows/677b5abc3b674d5b85eeba0825f3c38d) 。接着运行这个 Workflow，查询你想降级的 App.

![](http://7xibfi.com1.z0.glb.clouddn.com/uploads/default/optimized/2X/8/809f31c9556f693c469776643d6a37255a4691a3_1_690x306.jpeg)

选择查询版本信息，最后你会得到一个文本数据，使用 Airdrop 传到 Mac 上备用。
接着我去 Google 了一下，1Password 对于 iOS 7 最后的一个版本是 Version:4.5.3，打开刚才 iOS 那边发送过来的版本信息查询 Version:4.5.3 所对应的 ID：596285445。记好这个 ID 它就是 1Password Version:4.5.3 的“身份证”一会我们就要告诉 iTunes 去下载这个版本的 1Password。


![](http://7xibfi.com1.z0.glb.clouddn.com/uploads/default/optimized/2X/3/324c5836c55af57d1930e70b572d44d5d6204da4_1_680x500.png)



### 接下来打开 iTunes 与 Charles，前往 App Store ，查找 1Password 并下载。打开 Charles ,看看是否有一个地址是：p数字-buy.itunes.apple.com 的链接。


![](http://7xibfi.com1.z0.glb.clouddn.com/uploads/default/original/2X/c/cd814c64204c00c1076a2484d774b76514080d03.png)

右键打开 SSL 流量分析与断点。并且删除 iTunes 中正在下载的新版的 1Password。

![](http://7xibfi.com1.z0.glb.clouddn.com/uploads/default/optimized/2X/6/6f64e9f05f6142f02c79354cbb805a1d898f34c8_1_225x500.png)


在 iTunes 1Password 界面按下 ⌘ ＋R ，刷新下载页面，再次选择下载 1Password。当开始下载时就触发了 Charles 的断点，跳出了这样一个界面。选择 Edit Request -> XML Text 之后就会看到这样一个界面：

![](http://7xibfi.com1.z0.glb.clouddn.com/uploads/default/optimized/2X/5/56e9f1b711c839f1cc3c452d085f449db431e15a_1_690x486.png)


将 appExtVrsld 对应的值修改为刚才查询到的：596285445，然后点击 Execute，之后还有再跳出断点界面直接点击 Execute。
返回 iTunes 你就会看到我们的 1Password Version:4.5.3 正在下载了.

![](http://7xibfi.com1.z0.glb.clouddn.com/uploads/default/original/2X/f/f489768d06fb065d2fca73f221301f97380ae75d.png)

下载成功后再使用 iMazing 将 ipa 安装到我的 iPhone 4 上面，于是又可以开始折腾了。（之所以建议使用 iMazing 是因为如果使用 iTunes 有时还需要经过备份数据等操作耗时较久，其他盗版助手类工具不是太喜欢所以就是用 iMazing 了。）
![](http://7xibfi.com1.z0.glb.clouddn.com/uploads/default/optimized/2X/a/a480ba48de8925448fb564782a90834f454c567b_1_690x443.png)

![](http://7xibfi.com1.z0.glb.clouddn.com/uploads/default/optimized/2X/4/4868fa4858181c6f35d8befcae55d4ab98ad67d3_1_333x500.jpg)
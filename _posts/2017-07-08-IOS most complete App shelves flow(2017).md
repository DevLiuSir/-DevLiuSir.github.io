---
layout: post
title:  iOS 最全App上架流程(2017）
date:   2017-07-08 22:21:49
categories: 能工巧匠集
tags: 能工巧匠集
---

## 一、前言：
作为一名iOSer，把开发出来的App上传到App Store是必要的。下面就来详细讲解一下具体流程步骤。

## 二、准备：

1. 一个已付费的开发者账号（账号类型分: `个人（Individual）、公司（Company）、企业（Enterprise）、高校（University）`四种类型，每年资费分别为`$99、$99、$299、`免费。）。
2. 一个已经开发完成的项目。

## 三、检查：

1. 你的Xcode必须是正式版的，beta版本的Xcode是不能上传项目的。
2. 上传使用的 Mac 的 OS X系统必须也是正式版的，beta版本也不行。
3. 请确认你安装的Xcode是从App Store或者是开发者网站下载的，而不是从其它渠道获取的安装包安装的,因为非官方途径下载的Xcode可能带有`XcodeGhost` 病毒。如何检查？

![检查方法](http://upload-images.jianshu.io/upload_images/1400788-140d6ddefeae4be9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 四、生成发布证书

1.打开苹果开发者中心：[https://developer.apple.com](https://developer.apple.com)
打开后点击：`Account`


![苹果开发者中心](http://upload-images.jianshu.io/upload_images/1274138-aad4e32c1b9d9a0c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下面输入已付款过的Apple账号和密码登录（如果你的电脑已经保存了密码，会直接进入）


![开发者登录账号](http://upload-images.jianshu.io/upload_images/1274138-0fafe33f40e14999.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.点击：`Certificates`, `Identifiers & Profiles` (专门生成证书，绑定`Bundle Id`，绑定`device`设备，生成描述文件的地方)


![Member Center](http://upload-images.jianshu.io/upload_images/1274138-680fcbea31624001.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.点击Certificates生成证书
 
 1.选择iOS, tvOS, watchOS
 2.选择All
 3.点击右上角新添加证书

![添加新证书1](http://upload-images.jianshu.io/upload_images/1274138-6e167250ad616b18.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4.由于是做App上传，选择生产证书（选择App Store and Ad Hoc）


![选择App Store and Ad Hoc](http://upload-images.jianshu.io/upload_images/1274138-1b6916e4fc4b352c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 注意：一个开发者账号只能创建（1-2个开发（测试）证书，2-3个生产（发布）证书），如果你的App Store Ad Hoc 前面的按钮不能选择，则代表你的这个账号无法再创建新的生产证书了。

> 解决方法：
1.从共同使用这个账号的人电脑上生成.p12文件，导入自己的电脑。（尽量不要执行下面第2步）
2.如果你想生成的话，把现有的删除一个（建议删除时间比较靠前的）。注意：如果删除一个证书，那么正在使用这个证书的人将不能再使用了，除非重新生成，然后利用.p12重新导入自己的电脑里！
注意：如果你想删除证书，执行下面步骤，否则略过。

![删除证书](http://upload-images.jianshu.io/upload_images/1400788-e5ca61d73081babf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后接上上图，生产证书部分继续


![生成证书2](http://upload-images.jianshu.io/upload_images/1274138-880b2a6b2ef83dcf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![生成证书3](http://upload-images.jianshu.io/upload_images/1274138-aa0147aa160a8561.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![生成证书4](http://upload-images.jianshu.io/upload_images/1274138-6481befab0c6cea9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5.上传CSR文件去获取证书（CSR文件需要我们到本机钥匙串里去创建）

 1.在`Launchpad`的 (其他) 里面，点击 `钥匙串访问弹` 出如下界面

![其他](http://upload-images.jianshu.io/upload_images/1274138-ad0808d89178b859.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![钥匙串访问](http://upload-images.jianshu.io/upload_images/1274138-8876e60b1130c953.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.工具栏选择 **钥匙串访问** -> **证书助理** -> **从证书颁发机构请求证书...**

![请求证书](http://upload-images.jianshu.io/upload_images/1274138-f30c1698a6bc79e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![证书信息](http://upload-images.jianshu.io/upload_images/1274138-24facb0e48b85b4a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.将CSR文件保存到MAC磁盘的某个位置（这里我选择的是桌面，进行存储）

![存储证书](http://upload-images.jianshu.io/upload_images/1274138-4df2ae59e93d618f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![点击完成](http://upload-images.jianshu.io/upload_images/1274138-b3256c983968c9f4?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![CertificateSigningRequest.certSigningRequest 文件](http://upload-images.jianshu.io/upload_images/1274138-db714fde2cafeff9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6.然后回到浏览器，点击`choose File..`


![选择CSR文件](http://upload-images.jianshu.io/upload_images/1274138-870a2a1648e7964a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

7.选择创建好的：`CertificateSigningRequest.certSigningRequest` 文件，点击 **选取**


![选取CSR文件](http://upload-images.jianshu.io/upload_images/1274138-ec52a8da9c4d4080.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击 `Generate` 上传证书


![上传CSR证书](http://upload-images.jianshu.io/upload_images/1274138-0fc7782a44570072.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

8.跳转到如下界面，点击 `DownLoad` 下载生成的证书（cer后缀的文件），然后点击`Done`，你创建的发布证书就会存储在帐号中。


![下载生成的证书](http://upload-images.jianshu.io/upload_images/1274138-4542b7d35e10b9a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![cer后缀的文件](http://upload-images.jianshu.io/upload_images/1274138-15535ecf801c2233.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**注意**：这个证书只能下载一次。点击下载后，关闭页面后就不能再回到下载页面了。
如果不需要给别的电脑使用，则直接跳过下面附加项，跳转到第五步（绑定Bundle Identifier）

#### 附加项：生成p12文件在其他电脑上使用这个发布证书

1. 双击安装证书后，打开钥匙串访问，选择安装的证书右键单击

![安装的发布证书](http://upload-images.jianshu.io/upload_images/1274138-e4bfac9509e59734.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**注意**：如果没有**导出**，可以把这个证书删除，然后重新双击`下载的证书文件`安装。

1.导出证书

![导出证书](http://upload-images.jianshu.io/upload_images/1274138-2a98e6b41357d80a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.存储证书

![存储导出的证书](http://upload-images.jianshu.io/upload_images/1274138-abd22dd4006f4d65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**注意**：存储的文件格式一定要是`.p12`

3.设置密码

可以为证书设置密码，也可以不设置密码；如果设置了密码，那么别人安装这个证书的时候就要输入密码，否则无法安装。这里就不设置密码了。

![设置密码](http://upload-images.jianshu.io/upload_images/1400788-24b67b94cd2b6e6b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4.保存导出的证书

![p12发布证书](http://upload-images.jianshu.io/upload_images/1274138-85f8f52e0141a6d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果需要在其它电脑上也能发布App,那么就必须要安装这个发布证书。

## 五、创建App IDs和绑定你的App的Bundle Identifier

回到刚才的页面：[https://developer.apple.com/account/ios/identifiers/bundle/bundleList.action](https://developer.apple.com/account/ios/identifiers/bundle/bundleList.action)

1.点击App IDs,进入如下界面，点击右上角的 + 号


![点击App IDs](http://upload-images.jianshu.io/upload_images/1274138-6a6a88fdce4fed8f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


2.填写App IDs和Bundle Identifier


![填写App IDs和Bundle Identifier](http://upload-images.jianshu.io/upload_images/1274138-8510374b1d2e269d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 注意：

1.上传App使用的`Bundle Identifier`(不要有-，都是英文+数字)必须是固定的，不能使用占位符。

2.如果你的`Bundle Identifier`已经在网站上绑定了，如果你又修改了你工程里面这个`Bundle Identifier`的话，需要重新进入到开发者账号里面绑定。

![修改工程的Bundle Identifier](http://upload-images.jianshu.io/upload_images/1274138-883f55effaf72bb1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下面选择App中包含的服务，默认有两项，其余的根据自己项目的需求进行选择

![App Services](http://upload-images.jianshu.io/upload_images/1400788-234460360220541f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.点击`continue`


![点击continue](http://upload-images.jianshu.io/upload_images/1274138-52037637f6a2cc15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4.点击Register

![点击Register](http://upload-images.jianshu.io/upload_images/1274138-4c61fee1a2752901.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5.点击Done

![点击Done](http://upload-images.jianshu.io/upload_images/1274138-762cabdbfce24ca4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 六、生成描述文件（描述文件的作用就是把证书和Bundle Identifier关联起来）

1.找到`Provisioning Profiles` ，点击`All`，然后点击右上角 + 号


![Provisioning Profiles](http://upload-images.jianshu.io/upload_images/1274138-ef506981f65e7382.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.因为是发布，所以选择下面App Store这个描述文件，点击Continue


![选择App Store](http://upload-images.jianshu.io/upload_images/1274138-34d1fc1c18b8a951.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.在App ID 这个选项栏里面找到你刚刚创建的：`App IDs（Bundle Identifier`） 类型的套装，点击`Continue`

![选择发布项目的Bundle Identifier](http://upload-images.jianshu.io/upload_images/1274138-8d669ba0500397a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4.选择你刚创建的发布证书（或者生成p12文件的那个发布证书），根据自己电脑上的发布证书日期来选择，点击`Continue`


![选择创建的发布证书](http://upload-images.jianshu.io/upload_images/1274138-03caf3bb128101f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5.在`Profile Name`栏里输入一个名字（这个是PP文件的名字，可随便输入，在这里我用工程名字，便于分别），然后点击**Generate**

![给描述文件起个名字](http://upload-images.jianshu.io/upload_images/1274138-5038e40b6bb04861.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6.`Download`生成的PP文件，然后点击`Done`，双击安装（闪一下就完事了，没其它效果）

![Download生成的PP文件](http://upload-images.jianshu.io/upload_images/1274138-ab108d3df2888867.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![生成的描述文件](http://upload-images.jianshu.io/upload_images/1274138-e13c79c500072b05.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 七、在App Store开辟空间

1.回到	`Member Center`，点击 **`iTunes Connect`**


![点击iTunes Connect](http://upload-images.jianshu.io/upload_images/1274138-3542ac18c438720a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.登录开发者账号（还是之前已付费的账号）


![Snip20160315_68.png](http://upload-images.jianshu.io/upload_images/1274138-3f0a46b9e1857622.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.登录成功后，点击我的App


![点击我的App](http://upload-images.jianshu.io/upload_images/1274138-d32c6e593ee5bc3a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4.点击左上角那个＋号，点击新建(注意：我们是iOS App开发，不要选Mac App啦）


![新建 App](http://upload-images.jianshu.io/upload_images/1274138-829122d431cda775.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5.依次按提示填入对应信息（SKU是公司用于做统计数据之类的id，根据公司需求填写），然后点击创建


![填写App信息](http://upload-images.jianshu.io/upload_images/1274138-10c7a813d9da5d1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**注意**：如果都填好以后，可能会告诉你，你的App名称已经被占用，那么不好意思，你只能改名了！（而且建议大家起名不要往比较出名的App上靠，否则审核可能会被拒绝）

6.填写App其它信息


![App信息](http://upload-images.jianshu.io/upload_images/1274138-e67e0c17a3695489.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

7.填写价格和销售范围（由于我的开发者账号没有签订纳税合同，所以不能上线收费应用，所以只能暂时免费）


![填写价格和销售范围](http://upload-images.jianshu.io/upload_images/1274138-ebee35b184b94e77.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

8.依次把不同尺寸的App截图拉入到对应的里面
需要填写不同尺寸的手机屏幕截图（也就是拿不同尺寸的模拟器运行后，挑出至少3页最多5页进行截图然后拖到响应的区里）（在模拟器`Command＋S` 就可以保存屏幕截图到桌面了）（注意：如果提示拖进去的图片尺寸不对，则把模拟器弄成100%然后再`Command+S`) 尺寸参照表在下面


![设置不同尺寸的App截图](http://upload-images.jianshu.io/upload_images/1274138-c9cf71d6f9190ccb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

尺寸参照图

![尺寸参照图](http://upload-images.jianshu.io/upload_images/1400788-89426f47ccffa013.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

9.填写App简介

![填写App简介](http://upload-images.jianshu.io/upload_images/1274138-90a7666a002e9298.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

10.按提示依次输入


![按提示依次输入](http://upload-images.jianshu.io/upload_images/1274138-060b24c5decfdf45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**错误提示**：如果上传App 图标失败，提示Alpha错误的话，看下面。
打开你的图标图片，勾掉这个


![勾掉这个](http://upload-images.jianshu.io/upload_images/1274138-aabfd729a7294246.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

11.点击分级后面的编辑，如实填写后，点击完成


![分级信息](http://upload-images.jianshu.io/upload_images/1274138-436fa4098398c99a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

12.填写审核信息


![填写审核信息](http://upload-images.jianshu.io/upload_images/1274138-e94e6b5c37833d16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

版本发布就是：（然后最下面选择自动发布的话就是如果审核通过，就自动上传到App Store供人下载）

13.此时这个构建版本还没有生成，我们先把基本信息填写完毕，然后再进入Xcode中把项目打包发送到过来。

>注意：填写完一定要点击右上角的保存。

## 八、在Xcode中打包工程

找到你刚刚下载的发布证书（后缀为.cer）或者p12文件，和PP文件，双击，看起来没反应，但是他们已经加入到你的钥匙串中。

1.在Xcode中选择`iOS Device`(这里不能选择模拟器)，按照下图提示操作


![选择iOS Device](http://upload-images.jianshu.io/upload_images/1400788-64a16f618e2a6781.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.如果你的应用不支持横屏，把这两个勾去掉
![](http://upload-images.jianshu.io/upload_images/1274138-77f8992cb6708ea1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.查看版本号和构建版本号

![查看版本号和构建版本号](http://upload-images.jianshu.io/upload_images/1274138-3174a1e29a3c8ccd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4.配置发布证书(ps:截图时选择的证书是错误的，大家注意一下，要选择发布证书)


![配置发布证书](http://upload-images.jianshu.io/upload_images/1274138-6563365591deb1db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**注意**：如果这里没有黄色叹号，代表你的配置没问题，如果有，那就是证书和描述文件不匹配，或者描述文件里刚才选的`Bundle`和现在的工程的`Bundle Identifier`不一致，去`develop.apple.com` 上找到你的描述文件在确认下绑定的`bundle Identifier`和你工程是不是一样的？

![检查](http://upload-images.jianshu.io/upload_images/1274138-3c82083f108ae435.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5.将**断点**、**全局断点**，**僵尸模式**等都要去掉。


![去掉断点、全局断点](http://upload-images.jianshu.io/upload_images/1274138-88963e111facec1a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/1274138-53f15096a922ae60.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/1400788-f5a1db276fabf6c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![去掉僵尸模式](http://upload-images.jianshu.io/upload_images/1400788-2a88c6c1192d0d2e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6.设置**Release模式**（Debug是测试的，Release是发布用的）

![](http://upload-images.jianshu.io/upload_images/1400788-0aa405f4d5ead708.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/1400788-b61a9846cff4ee4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/1400788-dc9e0fff9473162e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/1400788-a5efd847c2fcfca2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


7.选择 `Xcode`下 `Product` 下 `Archive`（专门用于传项目，或者打包项目）

![选择Archive](http://upload-images.jianshu.io/upload_images/1274138-70b8ab0c2b9bb101.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/1274138-798bfadcd23603eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/1274138-edee87c38ab1f640.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

8.出现下图说明你没有添加开发者账号，点击右下脚Add... 按钮就可以添加


![没有添加开发者账号](http://upload-images.jianshu.io/upload_images/1400788-1e5412d2ef0dd03b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

9.输入付费的开发者账号


![输入开发者账号](http://upload-images.jianshu.io/upload_images/1274138-5b8c030f7cfe2f35.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可能会弹出下面这个界面，如果不弹出，按Command + 。
![](http://upload-images.jianshu.io/upload_images/1274138-cf42fa9539ab7c51.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

10.然后回到`Archive`(选择已付费的账号)，然后点击`Choose`


![选择已付费的账号](http://upload-images.jianshu.io/upload_images/1274138-5996025cf97df41e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后等待


![等待](http://upload-images.jianshu.io/upload_images/1400788-3095971312b9506a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

11.选择Upload提交

![Upload提交](http://upload-images.jianshu.io/upload_images/1274138-962feeb2f477f0e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

12.如下就代表上传成功，如果出错，请参照iOS App上传项目遇到的问题


![上传成功](http://upload-images.jianshu.io/upload_images/1400788-7ae4499c3860f795.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

13.返回`﻿﻿iTunes Connect`网站上你自己的App信息中查看一下


![在App信息中查看](http://upload-images.jianshu.io/upload_images/1274138-4c2ef528254c60ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

14.在这个构建版本这里就可以添加代码


![添加代码](http://upload-images.jianshu.io/upload_images/1400788-79c873cc4709c24c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击＋号之后选择代码版本

![添加构建版本](http://upload-images.jianshu.io/upload_images/1274138-28d9b6108694eae6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

15.提交以供审核


![提交以供审核](http://upload-images.jianshu.io/upload_images/1400788-dd5b07fe130a9194.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

16.App已经从准备提交，变成正在等待审核状态


![正在等待审核状态](http://upload-images.jianshu.io/upload_images/1274138-ab1b31a9096bc8c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

17.等待苹果的审核...

## 九、补充

>1、构建版本的方法有两种:
>
>	一种是使用 Xcode(上面已经介绍了)，
>
>	另一种就是使用一个叫 Application Loader 这个软件：


![Application Loader](http://upload-images.jianshu.io/upload_images/1274138-6ef9bca00ad25943.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- .使用Xcode构建版本：
上面已经介绍（已经`Archive`的话，可以在`Xcode: Window -> Organizer` 进入）


![Upload to App Store…](http://upload-images.jianshu.io/upload_images/1274138-a88ef12672e613de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 使用`Application Loader` 构建版本：

	- 如上图，点击`“Export...”，`导出一个ipa文件（ps:为了保险起见、让苹果审核顺利一点，这里可以先点击`“Validate…”`来验证一下，这里就懒得验证了）

	- `Application Loader`下载Xcode的时候就已经下载了（没有的请自行下载），Xcode 右键弹出下面界面

![Open Developer Tool](http://upload-images.jianshu.io/upload_images/1274138-30c74e2e70083411.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开`Application Loader`（如果没有登录，会提示你登录你的开发者账号）

![Application Loader](http://upload-images.jianshu.io/upload_images/1274138-54e104e615ba81d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选取你导出的ipa文件（或者双击“交付您的应用程序”选取ipa文件），之后按提示操作就行，上传完毕没有报错的话，过几分钟左右就可以在`iTunes Connect` 里面就可以看到你上传的这个版本了。

![构建版本](http://upload-images.jianshu.io/upload_images/1274138-28d9b6108694eae6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

个人建议构建版本使用 `Application Loader`上传!
---
layout: post
title:  使用Github + Jekyll搭建个人博客
date:   2017-01-08 22:21:49
categories: 能工巧匠集
tags: 能工巧匠集
---

![](http://upload-images.jianshu.io/upload_images/119048-12115e2953edd3be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

长大后才发现政府建造 GFW 真是太 TM 机智了，由于本人自制力较差，且不说 91porn, youporn 等两性知识网站的超强战斗力，单单一个Youtube就可以让我瞬间强撸灰飞烟灭。即使本人意志力足够强，万一不小心点击进了大纪元，遭受到了轮子思想的侵袭，动摇了我无条件支持 CCP 的 自干五 情怀那也是极其不好的。

下面问题来了，翻越 GFW 难道一无是处吗？
当然不是，可以努力的薅万恶的资本主义国家羊毛嘛。

下面是作者自建 Github 主页的详细过程，教程中部分网站需要 VPN 服务才能登陆，若无法登陆，你懂的。其次，作者使用的设备为 Mac，Windows 用户在部分内容上会有所出入。

## 一、使用 GitHub Education 福利

Google 输入 [github education](https://education.github.com/), 点击进入，如下图所示。
![](http://upload-images.jianshu.io/upload_images/119048-f0b024f2c447c0f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击 Get the pack，如下图。
![](http://upload-images.jianshu.io/upload_images/119048-3a538c2ec397e4f1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Get your pack 如下图。
![](http://upload-images.jianshu.io/upload_images/119048-e9133f583aa1f5cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接下来，按照网站的要求填写个人信息[你需要拥有校园网邮箱证明你的学生身份，才可以享受福利]。提交表单后，GitHub 需要1-2周的时间审核你的个人身份，然后你会收到这样一封邮件，受宠若惊有木有，反正当时我的内心是，还有服务这么好的公司。
![](http://upload-images.jianshu.io/upload_images/119048-9e7f19f52bd5dd7f.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Email Screenshot
点开邮件中的链接，你会来到 My pack 的界面，大礼包都在下面啦，可自行浏览。对 Github 个人主页最重要的礼包是 `namecheap` 一年免费域名。当然 `Digital ocean` 100刀的福利也是棒棒的，`ATOM` 编辑器也是写网站的不二选择。

![](http://upload-images.jianshu.io/upload_images/119048-9e2f7a35d804c328.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

有了域名礼包后，不急，过会我们才会用到它。

## 二、使用 GitHub Pages 建站

Google 搜索 `github pages`，点击进入，如下图。
![](http://upload-images.jianshu.io/upload_images/119048-1d2cd04a916d3dde.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

按照官网教程，不急，我们一步一步来。
### 官网第一步：


Create a repository
![](http://upload-images.jianshu.io/upload_images/119048-ba3bcf09237037bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

第一步，登陆 GitHub，新建一个 repository, 命名为 `你的用户名 + github.io`。如我的用户名为 `joeyqiang`，所以 `repository` 命名为 `joeyqiang.github.io`，如下图。
![](http://upload-images.jianshu.io/upload_images/119048-96e2f511eed2445a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 官网第二步：
![](http://upload-images.jianshu.io/upload_images/119048-f0b024f2c447c0f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我的电脑是 Mac, 选择 `GitHub for Mac`，然后将线上的 repository 克隆到本地 (我下载了 Github Desktop 用于同步，你也可以使用 Git 同步，看个人喜好)。
Clone 后，你会看到如下界面。

![](http://upload-images.jianshu.io/upload_images/119048-044ea6789f1aebbc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 官网第三步：

![](http://upload-images.jianshu.io/upload_images/119048-665030e9ea875ffc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

好吧，打开 ATOM 编辑器，新建 index.html，输入你心中此时此刻所想，保存，如下图。


![](http://upload-images.jianshu.io/upload_images/119048-fcebc29837935f11.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 官网第四步：


![](http://upload-images.jianshu.io/upload_images/119048-c5951ae139b6ebbb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开 GitHub Desktop, 提交更新。


![](http://upload-images.jianshu.io/upload_images/119048-0631e1c316602885.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 官网第五步：

![](http://upload-images.jianshu.io/upload_images/119048-803b34dc39fa5475.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开你的浏览器，输入你刚建的 repository 的文件名，比如我输入 `joeyqiang.github.io`。 Bang! You get it. 如下图。


![](http://upload-images.jianshu.io/upload_images/119048-50719227ba2aed85.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在我们可以绑定私人域名到上面啦。该上文的 Namecheap 登场了。利用 GitHub 礼包中的 unique link 链接到 Namecheap Education。

![](http://upload-images.jianshu.io/upload_images/119048-679ef730c844d6ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


输入你喜欢的域名，完成订单。

![](http://upload-images.jianshu.io/upload_images/119048-d2dbfa4f246b71a2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


注册 Namecheap，登陆你的帐号，点击 Domain List, 然后点击 MANAGE，如下图。


![](http://upload-images.jianshu.io/upload_images/119048-9f0b1d1a54c86656.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击 Advanced DNS。
![](http://upload-images.jianshu.io/upload_images/119048-757fd6bf08ed11fb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



管理 DNS。
![](http://upload-images.jianshu.io/upload_images/119048-860b7f66483522e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Manage DNS

添加 A Record，最终结果如下图。


![](http://upload-images.jianshu.io/upload_images/119048-4f7ebf64861cc419.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开 GitHub，新建一个 file，如下图。

![](http://upload-images.jianshu.io/upload_images/119048-2c9c151ca8477232.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

将 file 命名为 CNAME (全部为大写)，然后添加你的个人域名 (不要带 http 前缀)，保存，pull request 到 master, 如下图。


![](http://upload-images.jianshu.io/upload_images/119048-6e8e44e3d5193f8f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Edit the file

我们现在打开浏览器，输入你刚购买的域名，我的是joeyqiang.me，出现了，出现了，出现了，有木有！！！如下图。

![](http://upload-images.jianshu.io/upload_images/119048-f61ba40aa1423d7d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



## 三、使用 Jekyll 搭建个人博客

打开 terminal，安装 Ruby (Mac 已经预装了 Ruby)。可以输入
`ruby --version` 去验证是否安装，如下图。


![](http://upload-images.jianshu.io/upload_images/119048-4e904713f23c2038.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接下来，输入 `sudo gem install github-pages`，安装 `Jekyll (gem update github-pages` 命令可以用来更新 `Jekyll`，以免 Github 服务器更新导致网站本地和线上表现不同)，如下图。


![](http://upload-images.jianshu.io/upload_images/119048-5ba79c6b10943766.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

之后你需要在 master 下新建一个 file，命名为 Gemfile，输入

```
source 'https://rubygems.org'
gem 'github-pages'
```


如下图。


![](http://upload-images.jianshu.io/upload_images/119048-5c61df6a7872ea28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行 terminal，使用命令行移至 repository 根目录下 (也可以直接在 GitHub Desktop 下右键 repository 打开)，如下图。


![](http://upload-images.jianshu.io/upload_images/119048-623358b7518734ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/119048-321e5c94bf6cd3cb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

之后运行

```
bundle exec jekyll serve
```

如下图。


![](http://upload-images.jianshu.io/upload_images/119048-6482fa8db015f4ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下面，就可以使用 Jekyll 啦，本地测试在浏览器输入 `http://localhost:4000`即可 (本地测试结束后 commit to master 提交线上即可)，如下图。

![](http://upload-images.jianshu.io/upload_images/119048-b717a48ade8c8347.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 四、使用 Jekyll 模板个性化博客

首先建议大家看下 [Youtube Jekyll](https://www.youtube.com/watch?v=IINPHVVrF5Q&list=PLWjCJDeWfDdfVEcLGAfdJn_HXyM4Y7_k-) 这个系列教程，比阅读 [官方文档](https://jekyllrb.com/) 快捷容易多了，深入了解则需阅读文档了。我 Jekyll 也是才学，很多地方也在摸索中，欢迎多多交流。

### 以用户为中心的五维设计法则来指导设计思路。

- Strategy: 搭建博客的目的

	- 表达自己的想法
	- 锻炼自己的设计能力
	- 作为日后找工作的作品
- Scope：有哪些功能和特性

	- Post: Easy to read, Easy to search, Tags, Recommended List, Comments
	- Design: Modern Style, Blank Space, CSS3, UX, English Version, SEO ,Responsive Design
	- About: Self-introduction, Resume, Portfolio, Feedback

- Structure：如何组合功能和特性

![](http://upload-images.jianshu.io/upload_images/119048-27695195a73cd189.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- Skeleton：界面控件的位置
- Surface：美化界面，视觉设计


本人视觉设计能力很一般，借鉴别人居多，加以自己的改动。博客界面设计采用 Jekyll 主题 Lanyon。当然如果你愿意用 Axure 设计原型图，再用 Photoshop 或 Sketch 完成设计稿，那也是极好的。

个人经验：

善用 Google，搭建的时候难免碰到棘手的问题。前辈们基本上已经帮我们踏出了一条康庄大道，特此感谢 cellier & Ovilia。Google 一下，你想要的基本都有。
评论插件我使用的是 Disqus.
分析插件我使用的是 Google Analytics.
脑图使用的是百度脑图，比 Mindjet 好用多了。
博客文章使用的是 Markdown 语法，因为我简书就是用 Markdown 编辑的，所以直接下载保存到博客根目录下就好了。Markdown 请见簡書--有逼格的寫作。
好了，我的 GitHub 个人博客 —— Joey · an uxer blog 就是这样炼成的，欢迎大家访问，博客持续更新中。
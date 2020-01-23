---
layout: post
title:  Swift变量
date:   2015-03-08 22:24:49
categories: Swift
tags: Swift
---
<h2>Swift 变量</h2>

变量是一种使用方便的占位符，用于引用计算机内存地址。
Swift 每个变量都指定了特定的类型，该类型决定了变量占用内存的大小，不同的数据类型也决定可存储值的范围。
上一章节我们已经为大家介绍了基本的数据类型，包括整形Int、浮点数Double和Float、布尔类型Bool以及字符串类型String。此外，Swift还提供了其他更强大数据类型， Optional, Array, Dictionary, Struct, 和 Class 等。

接下来我们将为大家介绍如何在 Swift 程序中声明和使用变量。
<h3>变量声明</h3>

变量声明意思是告诉编译器在内存中的哪个位置上为变量创建多大的存储空间。
在使用变量前，你需要使用 <b>var</b> 关键字声明它，如下所示：

{% highlight swift %}

var variableName = <initial value>

{% endhighlight %}

<b>以下是一个 Swift 程序中变量声明的简单实例：</b>

{% highlight swift %}

import Cocoa
var varA = 42
print(varA)

var varB:Float
varB = 3.14159
print(varB)

{% endhighlight %}

以上程序执行结果为：

{% highlight swift %}

42
3.14159

{% endhighlight %}

<h3>变量命名</h3>

变量名可以由字母，数字和下划线组成。
变量名需要以字母或下划线开始。
Swift 是一个区分大小写的语言，所以字母大写与小写是不一样的。
变量名也可以使用简单的 Unicode 字符，如下实例：


{% highlight swift %}

import Cocoa

var _var = "Hello, Swift!"
print(_var)

var 你好 = "你好世界"
var 菜鸟教程 = "www.runoob.com"
print(你好)
print(菜鸟教程)

{% endhighlight %}

以上程序执行结果为：

{% highlight swift %}
Hello, Swift!
你好世界
www.runoob.com
{% endhighlight %}


<h3>变量输出</h3>

变量和常量可以使用<b> print</b>（swift 2 将 print 替换了 println） 函数来输出。
在字符串中可以使用括号与反斜线来插入变量，如下实例：

{% highlight swift %}

import Cocoa
var name = "菜鸟教程"
var site = "http://www.runoob.com"

print("\(name)的官网地址为：\(site)")

{% endhighlight %}

以上程序执行结果为：

{% highlight swift %}

菜鸟教程的官网地址为：http://www.runoob.com

{% endhighlight %}
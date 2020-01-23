---
layout: post
title:  PyCharm创建.py自动添加文件头注释
date:   2017-12-30 21:22:49
categories: Python
tags: Python
---

在pycharm使用过程中，对于每次新建文件的时候，关于代码编写者的一些个人信息快捷填写，使用模板的方式比较方便。

配置方法如下：

#### 1.打开`pycharm`，选择`File-Default Settings`

![](https://ws4.sinaimg.cn/large/006tKfTcly1g1ds0w3mpmj30dk0rowim.jpg)


#### 2.依次选择`Editor`--`Color&Style`--`File and Templates`--`Python-Script`，并在图中第5步编辑模板内容

![](https://ws2.sinaimg.cn/large/006tKfTcly1g1ds0vz5zkj31c10u0qbw.jpg)


#### 3.编辑内容

```python
#!/usr/bin/python3
# -*- coding: utf-8 -*-
# @Time    : ${DATE} ${TIME}
# @Author  : Shark
# @Site    : ${SITE}
# @File    : ${NAME}.py
# @Software: ${PRODUCT_NAME}
可用的预定义文件模板变量为：
$ {PROJECT_NAME} - 当前项目的名称。
$ {NAME} - 在文件创建过程中在“新建文件”对话框中指定的新文件的名称。
$ {USER} - 当前用户的登录名。
$ {DATE} - 当前的系统日期。
$ {TIME} - 当前系统时间。
$ {YEAR} - 今年。
$ {MONTH} - 当月。
$ {DAY} - 当月的当天。
$ {HOUR} - 目前的小时。
$ {MINUTE} - 当前分钟。
$ {PRODUCT_NAME} - 将在其中创建文件的IDE的名称。
$ {MONTH_NAME_SHORT} - 月份名称的前3个字母。 示例：1月，2月等
$ {MONTH_NAME_FULL} - 一个月的全名。 示例：1月，2月等

```

#### 结果如下：
![](https://ws3.sinaimg.cn/large/006tKfTcly1g1ds4qhgeij31000akjsi.jpg)





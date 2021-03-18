---
layout:     post
title:      Big Sur CTL
subtitle:   关于macOS更新到Big Sur后CTL出现问题
date:       2021-03-18
author:     zondie
header-img: img/2021-03-18/bg.jpg
catalog: true
tags:
    - mac
	- macOS
	- Big Sur



---

## 前言

前几天把mac系统更新了下，更新之后感觉出了好多bug..也似乎变慢了，中间某些软件还出现了崩溃的情况，如Docker，只能卸掉，但比较麻烦的是CTL(CommandLineTools)似乎出现了问题，如`git`,`gcc`等工具都不能用了，搜索了下找到了个解决方案。但不明觉厉，于是先记录下...

***

## 解决方案

默认时，`xcode-select`的路径为`/Applications/Xcode.app/Contents/Developer`，这在更新后使用原来的工具时会出现问题，如下：

![](https://zondie17.github.io/img/2021-03-18/1.jpg)

但实际上点击安装后还是无法使用，会出现下载一段时间后无法安装的情况，再次不多赘述。

之后查看[After upgrade to Big Sur git stopped working](https://developer.apple.com/forums/thread/666584)问题，得到解决办法

>I was having the same issue.
>I managed to get it fixed by running 
>
>`sudo xcode-select -switch /Library/Developer/CommandLineTools`

如回答中所述，在命令行输入上述命令，之后问题即可解决：

![](https://zondie17.github.io/img/2021-03-18/2.jpg)

可见此时`gcc`已可成功使用，另外路径变为了`/Library/Developer/CommandLineTools`。

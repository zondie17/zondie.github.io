---
layout:     post
title:      PS/LR on Mac
subtitle:   Macbook Pro上PS/LR及插件DR5的安装
date:       2022-05-12
author:     zondie
header-img: img/2022-05-12/bg.jpg
catalog: true
tags:
    - Mac
    - Software
    - PS
    - LR




---

<script type="text/x-mathjax-config">   MathJax.Hub.Config({     tex2jax: {       inlineMath: [ ['$','$'], ["\\(","\\)"] ],       processEscapes: true     }   }); </script>

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

## 前言

因为前段时间买了相机，三分钟热度还没过去，最近对于拍照和后期修图都比较有热情。于是在Mac上要安装下后期处理的相关软件，Photoshop和Lightroom，不过出于坚持做白嫖党的动力(购买一年属实有点贵)，开始在网上四处搜寻破解版。。

## Photoshop

首先自然是安装Photoshop，这个网上资源真的很多。开始时在byr上下载2021版，没有问题。之后偶然在macwk.com上发现了有2022版的，果断更新下。现在所用版本是2022 23.10，还是很流畅的。

<img src="https://zondie17.github.io/img/2022-05-12/ps.png" style="zoom:50%;" />

## Lightroom

LR相比于PS就没有那么多资源了，安装过程中出现了挺多问题。感觉至少下了十几个G的资源)首先byr上只有8.0的版本，过于老旧，在这个版本中曲线甚至都没有RGB三个通道的，只有一个整体的曲线调整。之后在b站上搜索了很多视频，下面的要不就是公众号收费，要不就不太彳亍。有一个9.0版的或许可以，但我百度网盘没充会员..最后也没下载完。macwk上有10.4版的LR，但可惜链接挂了..

最后山穷水尽时，发现在www.macat.vip上找到了免费的LR，感动。不过2022的v11.3似乎只支持M1；只能退而求其次安装了2021的v10.4版，虽然用起来似乎不是非常的稳定，不过目前看起来还算ok。

<img src="https://zondie17.github.io/img/2022-05-12/ps.png" style="zoom:50%;" />

## DR5

最后则是插件，DR5插件不是很难找，这里不做过多赘述。主要是激活部分，要签署成功需要在终端执行以下指令。

<img src="https://zondie17.github.io/img/2022-05-12/sign.png" style="zoom:50%;" />

然后20版本是`defaults write com.adobe.CSXS.10 PlayerDebugMode 1`，类推21是11，22是12。执行之后即可成功签署。
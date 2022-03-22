---
layout:     post
title:      Mac Wallpaper
subtitle:   MacBook下载(之前)版本系统壁纸
date:       2022-03-22
author:     zondie
header-img: img/2022-03-22/bg.jpg
catalog: true
tags:
    - Mac
    - Wallpaper




---

<script type="text/x-mathjax-config">   MathJax.Hub.Config({     tex2jax: {       inlineMath: [ ['$','$'], ["\\(","\\)"] ],       processEscapes: true     }   }); </script>

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

## 前言

最近看壁纸，感觉白天的Big Sur太亮，晚上又无生趣；傍晚时似乎很美，但在它的路径`/System/Library/Desktop Pictures`下又找不到`heic`格式的图片，在网上搜索只能找到$6016\times6016$格式的`jpg`文件...

## ".madesktop"

在该路径下只能找到一些`.madesktop`类型的文件，这些文件又不能够打开，搜索之后找到了一个相关回答：[What is a ".madesktop" file and how can I access its data?](https://discussions.apple.com/thread/253322672)

摘抄回答如下：

> That appears to be the pointer to the thumbnails since the desktops must be downloaded. 
>
> I think the downloaded images are in a uniqueID folder in:
>
> ```bash
> /System/Library/AssetsV2/com_apple_MobileAsset_DesktopPicture/
> ```

翻译即：

> 这似乎是指向缩略图的指针，因为必须下载桌面。
> 我认为下载的图像位于以下位置的 uniqueID 文件夹中：
>
> ```bash
> /System/Library/AssetsV2/com_apple_MobileAsset_DesktopPicture/
> ```

之后确实上述路径中找到了一个文件，即

```bash
com_apple_MobileAsset_DesktopPicture.xml
```

打开文件，发现其内容如下：

![](https://zondie17.github.io/img/2022-03-22/xml.png)

其中可以看到有两个网址，

```bash
http://updates-http.cdn-apple.com/2021/mobileassets/071-34545/7E2884C0-2F21-4028-B22E-2EA162B35135/
com_apple_MobileAsset_DesktopPicture/98b5bd24c8bb188ec6101ea415ec049fdd03de84.zip
```

将其拼接访问即可下载原`heic`格式文件。

## 后续

然而`/System/Library/Desktop Pictures`只读，用`sudo cp`指令复制壁纸失败后，干脆直接用`heic`转`jpg`的图片做背景了。

转完后发现格式也是$6016\times6016$...唯一有点价值的可能是可以选择无损转化，这样转完之后大小是`20Mb`左右，但我并没看出来和网上下载的图片有什么区别..
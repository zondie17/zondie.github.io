---
layout:     post
title:      about VSCode Error
subtitle:   关于FFT在VSCode上爆红的问题
date:       2021-03-12
author:     zondie
header-img: img/2021-03-12/bg.jpg
catalog: true
tags:
    - VSCode


---

## 前言

最近在做毕设，要在服务器上修改fft代码，然后在本地VSCode上修改代码的时候，总是会莫名其妙的报错，感觉很迷，于是调试(魔改)了下，然后好了...在这里记录下，倒不一定是之后会用到经验，只是万一改崩了能想起来是改了哪...

***

## 解决方案

未修改时，显示语法高亮错误，但能够成功编译，报错如下图：

![](https://zondie17.github.io/img/2021-03-12/1.jpg)

之后查看[Visual Studio Code Syntax Highlighting shows errors but compiles](https://stackoverflow.com/questions/47429752/visual-studio-code-syntax-highlighting-shows-errors-but-compiles)问题，得到解决办法

>Setting the `"C_Cpp.intelliSenseMode": "Tag Parser"` in the `settings.json` does the trick. 

具体来说，我是在这个项目`./.vscode`路径下修改了`settings.json`，另外通过`command+shift+p`直接键入`settings.json`也可找到，直接添加一句，修改如下：

![](https://zondie17.github.io/img/2021-03-12/2.jpg)

即可，之后问题解决。

另外，搜索得到如下解释：

>`browse` The set of properties used when `"C_Cpp.intelliSenseEngine"` is set to `"Tag Parser"`(also referred to as "fuzzy" IntelliSense, or the "browse" engine). These properties are also used by the Go To Definition/Declaration features, or when the "Default" IntelliSense engine is unable to resolve the #includes in your source files.

来自[c_cpp_properties.json reference](https://code.visualstudio.com/docs/cpp/c-cpp-properties-schema-reference) .

***

## Others

另外可以修改右下角从右向左第四个显示`C`的地方，修改语言模式。

![](https://zondie17.github.io/img/2021-03-12/3.jpg)


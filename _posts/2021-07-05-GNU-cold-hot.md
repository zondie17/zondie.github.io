---
layout:     post
title:      GNU cold hot
subtitle:   GNU中attribute中的cold和hot
date:       2021-07-05
author:     zondie
header-img: img/2021-07-05/bg.jpg
catalog: true
tags:
    - GNU
    - attribute



---

## 前言

看项目时看到了这个语法`define av_cold __attribute__((cold))`不知道什么意思。

搜索后感觉蛮有趣的..就记录下。

## GNU中attribute中的cold和hot

cold

> The cold attribute on functions is used to inform the compiler that the function is unlikely to be executed. The function is optimized for size rather than speed and on many targets it is placed into a special subsection of the text section so all cold functions appear close together,improving code locality of non-cold parts of program. The paths leading to calls of cold functions within code are marked as unlikely by the branch prediction mechanism. It is thus useful to mark functions used to handle unlikely conditions, such as perror, as cold to improve optimization of hot functions that do call marked functions in rare occasions. 

hot

> The hot attribute on a function is used to inform the compiler that the function is a hot spot of the compiled program. The function is optimized more aggressively and on many targets it is placed into a special subsection of the text section so all hot functions appear close together, improving locality. 




---
layout:     post
title:      RISC-V Notes
subtitle:   RISC-V r3/5/7的一些bug笔记
date:       2022-03-29
author:     zondie
header-img: img/2022-03-29/bg.jpg
catalog: true
tags:
    - RISC-V
    - Programming
    - bug





---

<script type="text/x-mathjax-config">   MathJax.Hub.Config({     tex2jax: {       inlineMath: [ ['$','$'], ["\\(","\\)"] ],       processEscapes: true     }   }); </script>

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

## 前言

前两天又继续开发了下RISC-V上的OpenFFT，好久不见当然也记不得多少了。花了两天重温自己之前写的shit一样的代码后，继续完成基-3/5/7的汇编开发，这次没啥结构追求，直接快速赶完。整体来说还比较顺利，中间稍微出了几个小问题，记录下debug的历程。

## Segment Fault

编译过程中有两次报Segment Fault的错误，一次基-3，一次基-5；原因都是函数传参时候的问题。传参错误，所以会访问到不该访问的地方..

### 基-3

基-3时传参数时直接模仿arm汇编代码写的，如图。

<img src="https://zondie17.github.io/img/2022-03-29/r3-arm.png" style="zoom:50%;" />

只是把其中`x0`等寄存器改成了`a0`等寄存器（因为RISC-V函数传参寄存器默认是`a0-a7`共八个），`v0`保持不变，然后就寄了。arm这里之所以可以这样写，是因为浮点参数可以直接传到向量寄存器中，也就是`v0`。但risc-v里是不可以的...不管什么样的参数，都是会挨个传到`a0-a7`中。故其正确传参如下：

<img src="https://zondie17.github.io/img/2022-03-29/r3-risc-v.png" style="zoom:50%;" />

注意传的是浮点数，所以在之后计算中使用`fmv.w.x`或`fmv.w.x`进行一个类型转换。

### 基-7

基-7时又一次遇到了传参问题，这次是因为传参太多，不知道怎么整了..基-7传参arm汇编有9个，超出了`a0-a7`8个范围，如下图：

<img src="https://zondie17.github.io/img/2022-03-29/r7-arm.png" style="zoom:50%;" />

之后第一反应非常愚蠢的以为超过这8个之后，应该会传到其他寄存器里，于是用`gdb`看哪个寄存器里有参数，都试了试..然后才想起来google一下，顺利找到了本科学过的内容——参数过多会通过栈来传递。于是正确传参如下：

<img src="https://zondie17.github.io/img/2022-03-29/r7-risc-v.png" style="zoom:50%;" />

另外在代码区，`SAVE_REGS_RISCV64`之前将栈上的内容读入到对应寄存器中：

```assembly
lw      		butterfly_num, 0(sp)
```

## Illegal Instruction

这里主要是粗心的问题。。

报illegal的主要都是寄存器的重命名和汇编指令对应类型出了问题，前者经常是`ctrl+c`和`ctrl+v`的时候忘了修改名字，或者是一个参数在多个文件中出现，没有全部修改这种；后者则是像矢量标量之间运算时，没有正确使用对应的指令，或者参数之间的顺序出现了问题（如`vfmacc.vf`）。

```assembly
vfmacc.vf		vd, fs1, vs2
```

该指令严格要求标量必须放在中间..而`vfmul.vf`的标量则必须放最后：

```assembly
vfmul.vf		vd, vs2, fs1
```

## Error

计算错误主要也是粗心吧。。

首先由于代码重复度很高，存在大量`ctrl+c`和`ctrl+v`的情况，所以修改时经常会出现遗漏的情况，这种就很可能导致计算错误..所以慎之又慎。

另外是不同ISA之间指令神奇的对应关系，在arm中的`fmls`对应risc-v里的`fnmsub`...

```assembly
fnmsub.s		fd, fs1, fs2, fs3
vfnmsub.vf		vd, fs1, vs2
vfnmsub.vv		vd, vs1, vs2
```

确实，risc-v里不仅只有这个乘累减取负指令，对应arm里的`fmls`;还睿智的没有覆盖被乘数的浮点乘累减取负指令，所以单独算浮点数的时候，需要四个参数。上述三个指令的对应含义如下：

```c
fd 		= 	-(fs1*fs2 - fs3) = fs3 - fs1*fs2
vd[i] 	= 	-(fs1*vd[i]) + vs2[i]
vd[i] 	= 	-(vs1[i]*vd[i]) + vs2[i]
```

## ext

最后是ext这个神奇的缩写。。

在arm中，这通常是指Extraction instructions..例如用的很多的`ext`指令：

```assembly
EXT 			v0.16B, v1.16B, v2.16B, #3
```

这是提取两个寄存器中的数据进行组合的意思。对此来说，则是提取v1的低3位到v0的高位，提取v2的高13位到v0的低位。官方文档解释如下：

> 1. Copy the lowest 3 bytes from V1 into the highest 3 bytes of V0.
> 2. Copy the highest 13 bytes of V2 into the lowest 13 bytes of V1.

如下图：

<img src="https://zondie17.github.io/img/2022-03-29/reorder_ext1.png" style="zoom:67%;" />

但在risc-v中，通常是指的extension...

且在RISC-V中并没有找到如此灵活的提取指令（就显得很僵硬。

也可能是我没看到，之后如果有新发现的话会update的(吧)。
title: 在x86机器的屏幕上显示的三种方法

date: 2014-01-25 22:55:33

categories:
- 操作系统开发

tags:
- 操作系统开发
---

在实现x86操作系统的时候，肯定要在屏幕上显示字符、图形和图像，我个人总结，有三种在屏幕上显示的方法。

<!-- more -->

## 1. 调用BIOS中断

将数据写入内存，将内存指针存入CPU寄存器，调用中断。实模式下使用。相比较第二种方法的好处是，BIOS自带英文字库，编程简单。使用汇编实现。最终BIOS肯定是将数据发送到了显卡上的显存（帧缓存）上。

## 2. 向显存里直接写数据

也就是所谓的“直接写屏”。

实模式和保护模式下都可以使用，但是只有640KB。这640KB是和内存统一编址的，所以实际上这段物理内存被屏蔽了。通常用C语言实现。超过640KB的部分需要使用第三种方法。最终写到显卡的显存上。

## 3. 向显卡外设端口写指令和数据

这也是平时我们在使用电脑时的方法，当然，指令和数据是由应用程序发出的。保护模式下使用。最终由GPU处理后写到显存上。

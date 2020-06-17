title: x86系统引导

date: 2010-04-29 20:08:00

categories:
- 操作系统开发

tags:
- 操作系统开发
---

电脑加电后，BIOS里的程序先运行，装入硬盘的第一个扇区（512B），这里就是MBR，包括硬盘分区表和引导程序。

引导程序引导到逻辑盘里的操作系统引导程序。
也可以把操作系统引导程序（例如GRUB）放到MBR里，省去一个步骤。这就是为什么GRUB可以装在MBR里，也可以装载到逻辑分区里。

<!-- more -->

GRUB负责引导Linux内核源代码arch/i386/boot/里的汇编代码写的启动程序，这个启动程序再启动内核。

BIOS->GRUB->初始化程序
没有GRUB的话，BIOS->初始化程序

bootsect还是16位实模式，在Setup中进行保护模式。
title: 如何将python脚本转换成在Windows系统的可执行程序exe

date: 2013-08-24 17:11:00

categories:
- Python

tags:
- Python
- pyinstaller

---

截止到2012年12月，将python脚本转换成exe的最好的工具是pyinstaller

<!--more-->

### 1

下载python，可以下载2系列的，也可以下载3系列的，安装。

下载pywin32（请使用搜索引擎，官方网站在sourceforge上）,下载对应于python的版本号，以及电脑CPU架构（32位或64位）的版本，安装。

### 2

如果pywin32安装不成功，可以卸载掉python和pywin32，然后下载另外一个版本号的版本

### 3

下载pyinstaller（请使用搜索引擎，官方网站在sourceforge上），解压缩到某个目录，例如/to/your/path/pyinstaller-2.0

### 4

确保python脚本，例如 test.py可以正常执行，无错误。

### 5

将test.py放到/to/your/path/pyinstaller-2.0目录下

### 6

在/to/your/path/pyinstaller-2.0目录下执行python pyinstaller.py –-onefile test.py （注意onefile前面有两个-符号）

### 7

test.exe将生成在/to/your/path/pyinstaller-2.0/test/dist/目录下

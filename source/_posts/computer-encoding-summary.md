title: C语言字符编码的一点总结

date: 2010-04-10 19:03:00

categories:
- C和CPP

tags:
- 计算机编码
- utf
- 宽字符

---

wchar_t 其实只是对应于UTF-16的，也就是UCS2，但是编译器一般实现为4个字节

<!-- more -->

Linux下广泛使用UFT-8，UTF-8并不是宽字符，而是多字符

Unicode并不等于宽字符，UTF-16才是宽字符

wout输出要先设置locale，是因为要进行宽字符到多字符的转换，多字符的现实，需要指定活动代码页


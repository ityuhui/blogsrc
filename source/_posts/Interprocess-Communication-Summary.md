title: "进程间通信总结"
date: 2018-12-29 08:01:30
tags:
---

（远程）进程间通信的常用方法有两个（共享内存、管道、信号这些不提了）：
1. 远程过程调用 RPC，实现上有google的gRPC
2. 消息队列,实现上有各MQ

web服务的主流现在就剩下了两个：
1. RESTful
2. SOAP

gSOAP有两个工具
1. soapcpp2, 根据h文件，生成代码，以及wsdl
2. wsdl2h，根据wsdl或者xsd, 生成h文件，给soapcpp2使用

可以手写h文件，直接用soapcpp2


----------

线程间通信
信号量
锁

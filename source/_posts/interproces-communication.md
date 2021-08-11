title: 进程间通信

date: 2020-10-01 20:16:17

categories:
- 软件开发通用知识

tags:
- 操作系统
- 进程间通信
- interprocess communication
---

在操作系统原理里，进程间通讯有几种方法： 
* 信号
* 管道
* 共享内存
* 消息队列
* socket

在软件开发中，常用最后一种方法，就是通过网络，但是网络又有几种方法：

* REST
* SOAP
* Message Queue
* gPRC
* 自己实现socket server,socket client,自己定义消息

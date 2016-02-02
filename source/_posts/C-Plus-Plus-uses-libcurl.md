title: "C++ 使用 libcurl"
date: 2016-02-02 03:22:40
tags:
---
# 简介
使用C++编写http客户端程序，主要有下面两个方法：
> * **socket**
自己组装http包，向server的80端口发起请求，接收响应，处理。
> * **http library**
使用libcurl库，其他知名的还有boost::asio,ACE




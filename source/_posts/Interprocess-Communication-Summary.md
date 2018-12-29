title: "远程进程间通信总结"
date: 2018-12-29 08:01:30
tags:
---

# 远程进程间通信的常用方法有两个：
1. 远程过程调用 RPC，实现上有google的gRPC
2. 消息队列,实现上有各种消息队列（MQ）

# 作为远程进程间通信的一种实现形式，web服务的主流现在就剩下了两个，实际上都是PRC
1. RESTful
2. SOAP

# gSOAP是SOAP的C/C++实现，它主要有两个工具

## 1. wsdl2h，根据wsdl或者xsd, 生成h文件，给soapcpp2使用

## 2. soapcpp2, 根据h文件，生成代码，以及wsdl

- XML serializers for the data types (soapStub.h, soapH.h, and soapC.cpp), 
- the client-side stub functions (soapClient.cpp), 
- server-side skeleton functions (soapServer.cpp).

## 3. 也可以手写h文件，直接用soapcpp2


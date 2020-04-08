title: "C++ 使用 libcurl"

date: 2016-02-02 03:22:40

categories:
- 网络编程

tags:
- C++
- libcurl

---
# 简介
使用C++编写http客户端程序，主要有下面两个方法：
> * **socket**
自己组装http包，向server的80端口发起请求，接收响应，处理。
> * **http library**
使用libcurl库，其他知名的还有boost::asio,ACE，目前，libcurl的应用比较广泛。

<!--more-->

# 获得libcurl

## 到官方网站下载源代码包到本地
例如我的目录 $home/opt/libcurl

## 编译源代码
> ./configure
> make

得到静态库 libcurl.a
其实也得到了动态库，但是为了简单，我没有使用。

## 拷贝头文件
将include/curl目录拷贝到/usr/local/include下面

## 其实OSX系统自带libcurl
/usr/lib/libcurl.dylib，也可以链接使用

# 使用libcurl

## 创建工程，将 libcurl.a拷贝到此工程目录下

https://github.com/ityuhui/mycurlsample

## 编写程序

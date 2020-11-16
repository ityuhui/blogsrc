title: libcurl使用中的一些总结

date: 2020-11-16 21:15:07

categories:
- 网络编程

tags:
- libcurl
---

## 1. 终止一个永远连接的watch线程

不要试图使用pthread_cancel()，会发生资源泄露。

正确的方法是，使用一个加锁的变量，一个线程设置变量为“退出”，另一个Watch线程检查这个变量，然后在本线程内终止curl_easy_perfrom()

<!-- more -->

## 2. 如何终止curl_easy_perform()

有两种方法：

#### 第一种 使用过程函数

progress function 如果返回不为0，传输会终止。

[参考1](https://curl.se/mail/lib-2007-09/0097.html)

#### 第二种 重新设置超时

首先保存curl_handler
```c
curl_easy_setopt(curl_handler, CURLOPT_TIMEOUT_MS, 1)
```
然后传输就会终止。

[参考](https://stackoverflow.com/questions/28767613/cancel-curl-easy-perform-while-it-is-trying-to-connect)


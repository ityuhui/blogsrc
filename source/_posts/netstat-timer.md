title: netstat --timer命令的输出的意义
date: 2021-09-13 21:12:28

categories:
- Linux

tags:
- linux
- netstat
- keepalive
---

## 介绍

可以使用`netstat -o`或者`netstat --timer`来查看socket的keepalive

TCP keepalive 是 TCP协议提供的连接保活机制。有三个参数：
- /proc/sys/net/ipv4/tcp_keepalive_time
默认: 7200 秒
- /proc/sys/net/ipv4/tcp_keepalive_probes
默认: 9
- /proc/sys/net/ipv4/tcp_keepalive_intvl
默认: 75 秒

<!--more-->

## 举例
```shell
# netstat -tpnoe | grep pro1
tcp        0      0 Local:39413     Foreign:50508     ESTABLISHED 0          44513298   1510/pro1             keepalive (151.09/0/0)
```

## keepalive (151.09/0/0)
<第一部分> <第二部分>

## 第一部分的取值:
- keepalive：当该socket的keepalive计数器被打开。这个时候连接上没有应用程序的数据传输，只有TCP协议自己的保活消息。
- on：当该socket的retransmission计数器被打开。这个使用，应用程序在连接上尝试传输数据。
- probe: 零窗口探测计时器（连接计时器）
- off：以上计数器都没有被打开

## 第二部分有三个子部分:
(151.09/0/0) -> (a/b/c)

### 当第一部分 = keepalive 
- a
  * 当连接正常时，a=tcp_keepalive_time倒计时
  * 当tcp_keepalive_time倒计时为0并且连接不通时，a=tcp_keepalive_intvl倒计时
- b=没有用处
- c=已经测探侧次数计数器，对应于tcp_keepalive_probes

### 当第一部分 = on
- a=retransmission倒计时
- b=retransmissions计数器
- c=没有用处

## 参考
https://superuser.com/questions/240456/how-to-interpret-the-output-of-netstat-o-netstat-timers


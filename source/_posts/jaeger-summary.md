title: Jaeger

date: 2020-11-23 21:05:17

categories:
- 大数据和分布式计算

tags:
- jaeger
- 分布式追踪

---

## 介绍

Jaeger是一个分布式追踪系统，用于追踪调用链。

<!-- more -->

## 组成

Jaeger client: 应用程序里插入Jaeger库提供的函数，就成为了Jaeger client。当调用库函数的时候，向Jaeger agent发送信息。

Jaeger agent: 将收到的信息转发给Jaeger collector或者Jaeger backend

Jaeger collector: 将收到的信息转发给Jaeger backend

Jaeger backend: 将收到的信息汇总保存

Jaeger UI: 展示汇总追踪信息

All in one: 将上面除了Jaeger client之外的所有组件都放在一个应用程序里，用于开发和测试。

## 使用All in one快速开始

1. 启动all in one

```shell
jaeger-all-in-one --collector.zipkin.http-port=9411
```

2. 访问```http://localhost:16686```即可看到Jaeger GUI

## 库

Jeager提供了用于各种编程语言的库

例如
[Python client](https://github.com/jaegertracing/jaeger-client-python)




title: 使用libyaml

date: 2020-03-30 21:52:19

categories:
- C和CPP

tags:
- libyaml

---

## 介绍

libyaml是用于解析和生成yaml文件的C语言库，是yaml官方推荐的C语言库之一。

<!--more-->

## 读取

本文只关注于读取yaml文件，没有涉及生成和修改。

libyaml支持三种读取模式：

### 基于token

我个人觉得已经可以被基于event的模式取代

### 基于event

这是官方页面的实例代码介绍的模式，应用程序处理各种yaml定义的事件

- STREAM-START
- STREAM-END
- DOCUMENT-START
- DOCUMENT-END
- ALIAS
- SCALAR
- SEQUENCE-START
- SEQUENCE-END
- MAPPING-START
- MAPPING-END

来读取yaml文件内的元素

- stream ::= STREAM-START document* STREAM-END
- document ::= DOCUMENT-START node DOCUMENT-END
- node ::= ALIAS | SCALAR | sequence | mapping
- sequence ::= SEQUENCE-START node* SEQUENCE-END
- mapping ::= MAPPING-START (node node)* MAPPING-END

这种方法需要自己实现一个状态机，根据事件来判断下一步处理的事件，同时读取元素。

### 基于document

使用这种模式，libyaml将整个yaml读入内存，应用程序不需要再处理上面两种模式里的事件或者token, 只需要按照libyaml在内存中的数据结构安排，将其遍历出来，比较方便，也类似于libxml的模式。

使用这种模式，我实现了读取kubeconfig yaml文件，代码在

[kubeyaml](https://github.com/ityuhui/libkubeyaml)

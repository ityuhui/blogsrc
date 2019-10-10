title: 使用kubebuilder创建kubernetes的operator
date: 2019-10-06 17:04:20
tags: 
 - cloud
 - k8s
 - kubernetes
---
## 介绍
在kubernetes（以下简称k8s）里，operator指的是由CRD和controller共同构成的某项业务。CRD负责表示业务数据，controller负责业务操作（对业务数据的修改），两者共同完成某项业务在k8s里的运营。

创建CRD不需要编写程序，只需要写yaml文件，然后使用kubectrl命令部署到k8s里面就可以了，CRD部署到k8s之后，数据是存储在etcd里面的，只能手工（例如使用kubectrl）查询和修改，并没有什么实际作用，要想自动完成实际的业务，需要controller来实现。

创建controller需要编程，controller的基本的流程是：
 1. 监听CRD的变化
通过向API server放置watch/informer，当CRD发生变化，API server会通知controller
 2. 操作
根据业务需要，对获得的CRD或者k8s里的其他资源进行修改
 3. 写回
将变更的CRD信息写回API server

其中，第一步和第三步都可以通过REST操作来完成，所以理论上使用任何的编程语言都可以编写controller，但是，k8s社区已经把这些操作都封装成了各种语言的包来调用，省去了我们直接操作REST的不方便（特别是鉴权），在这些语言的包里，最推荐的无疑是k8s的原生语言golang编写的client-go

虽然有了client-go，我们还是需要自己编写很多的与具体业务无关的基础框架代码，例如监听CRD变化，写回状态，以及编写CRD的yaml，为了加快Operator的编写，k8s社区提供了kubebuilder，它可以为我们生成基础框架代码和CRD的yaml，我们只需要填写业务的数据成员和业务代码就可以了。

## 安装

### 安装 kuberbuilder

### 安装 kustomize

## 使用

### 创建

### 创建API

### 开发运行

### 部署

## 参考
[使用kubebuilder开发kubernetes CRD](https://segmentfault.com/a/1190000019892302)







title: kubernetes scheduler 介绍
date: 2020-04-03 23:09:30
categories:
- kubernetes
tags:
- kubernetes
- k8s
---

## 一、什么是 kubernetes scheduler

kubernetes scheduler 是 kubernetes 的核心组件，负责给需要执行的pod选择合适的node。

<!-- more -->

## 二、kubernetes scheduler 工作流程

kube-scheduler在API server处留有Watcher, 当有新的pod请求到达后，kube-scheduler查看pod的spec里是否指定了执行的node, 如果有，就忽略这个pod, 如果没有，就为这个pod启动调度流程，找到一个执行节点，将其回填回API server的pod信息里。

各节点上的kubelet从API server处观察到有pod的执行节点是自己所在的节点的时候，就在自己所在的节点上，创建并执行pod。

具体的调度（查找合适的执行节点）过程如下

### 第一阶段：预选

预选的作用，是找到满足条件的节点，如具有SSD硬盘，系统内存大于某个值，去掉不满足条件的节点。以下是几个比较重要的策略：

- 防止过度提交

- 反亲和

- 亲和

- 污染和容忍

### 第二阶段：优选

预选可能找到多个满足条件的node, 优选阶段将按照一些规则对其进行打分并汇总，打分高者最后会被选中。以下是几个优选的策略：

- 节点漫延

- 反亲和

- 亲和

打分后线性相加，得到最后的总分，分高的node将会被选中。

## 三、kubernetes scheduler 源代码分析

kubernetes scheduler 是一个单独的进程，但是从代码逻辑上，分为两个部分：cmd和pkg

### cmd

接收命令行参数

### pkg

调度的核心代码

## 四、如何扩展scheduler

有的时候，k8s 自带的kube-scheduler无法满足我们自己的需求，这时我们需要来扩展scheduler，目前，扩展schedule有以下几种方式：

### 1. 编写自己的sheduler, 与kube-scheduler共存

优点：自己有很大控制权。

缺点：不能利用到kube-scheduler已有的逻辑，需要自己从头写。

### 2. 使用externder/webhook

[参考](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/scheduling/scheduler_extender.md)

优点：可以利用kube-scheduler里的已有的逻辑

缺点：http连接可能会有性能问题； 只能编写一个扩展器，插入到一个地点，无法做到多个扩展共同生效。

### 3. 使用scheduler framwork

[参考](https://github.com/kubernetes/enhancements/blob/master/keps/sig-scheduling/20180409-scheduling-framework.md)

kuber-scheduler提供的扩展框架，可以编写我们自己的插件，将其插入kube-sheduler的执行流程，是未来主要的扩展方式。


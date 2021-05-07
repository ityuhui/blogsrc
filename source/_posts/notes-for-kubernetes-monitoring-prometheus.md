title: 《使用Prometheus监控Kubernetes终极指导》阅读笔记

date: 2021-05-07 20:13:01

categories:
- kubernetes

tags:
- prometheus
- kubernetes

---

## 介绍

本文是我在阅读 sysdig 官网上的文章 [Kubernetes monitoring with Prometheus, the ultimate guide](https://sysdig.com/blog/kubernetes-monitoring-prometheus/) 时所作的笔记。

## 原文主要内容
- Prometheus 核心概念
- 与其他监控方案的比较
- 如何安装
- 监控 Kubernetes Service
    * Prometheus exporters
- 监控 Kubernetes 集群
    * Kubernetes 内部 services
    * Kubernetes 节点
    * Kube State Metrics
    * Kubernetes 控制面

<!--more-->

## 原文的一些要点

### 为什么要使用 Prometheus 对 Kubernetes 进行监控：
- DevOps 需要
- 容器和 Kubernetes 的特点

### 为什么是 Prometheus
- 多维的数据模型：基于key-value键值对
- 可访问的格式和协议：Metrics 具有自我解释的格式，方便人读取；对外使用标准的 HTTP 传输。
![pict](https://478h5m1yrfsa3bbe262u7muv-wpengine.netdna-ssl.com/wp-content/uploads/Blog-Kubernetes-Monitoring-with-Prometheus-2-Prometheus-metrics-endpoint.png)
- 服务发现：Prometheus 服务器定期抓取目标数据，数据源不需要发送数据（ Metrics 是被 Prometheus 拉取的，不是自己推出的）
- 模块化和高可用性的组件

### 监控 containers ：可见性

- Kubernetes API 和 kube-state-metrics 对外暴露 Kubernetes 的内部数据，例如 deployment 的 副本数量，不可调度的 nodes 等等。
- 微服务只需要在现有的 HTTP 接口上增加 `/metrics` 路径。
- 如果你不能修改数据源所在的程序的代码，那么可以部署一个 Prometheus exporter 来转换 metrics, 通常是在同一个 pod 里以 sidecar container 的形式。
 
### 动态监控：不停的改变和校验 infrastructure
- Prometheus 具有一些自动发现机制，主要是使用一些服务发现：
    * Consul
    * Kubernetes Service Discovery
    * Prometheus Operator

---

### 架构预览

![pict](https://478h5m1yrfsa3bbe262u7muv-wpengine.netdna-ssl.com/wp-content/uploads/Blog-Kubernetes-Monitoring-with-Prometheus-4-Architecture-Overview.png)

#### 1. Prometheus 服务器需要尽可能多的自动发现目标：
- Prometheus Kubernetes SD (service discovery)
- Prometheus operator and CRD
- Consul SD
- Azure SD for Azure VM
- GCE SD for GCP instances
- EC2 SD for AWS VM
- File SD
- ...

#### 2. Kubernetes services, nodes, and orchestration status：
- 用 Node exporter 来收集经典的主机的指标：CPU, 内存，网络
- 用 Kube-state-metrics 来收集集群级的指标：deployments, pod metrics, resource 保留等等。
- Kubernetes 控制面指标: kubelet, etcd, dns, scheduler, 等等。

#### 3. Prometheus 可以使用 PromQL 来配置激发告警的规则

#### 4. `AlertManager` 组件配置接收者和网关来发送告警通知

#### 5. Grafana 从 Prometheus 服务器拉取数据来显示

---

### 如何安装 Prometheus

- 在你的主机上运行单独的二进制程序：

```shell
prometheus-2.21.0.linux-amd64$ ./prometheus
./prometheus
level=info ts=2020-09-25T10:04:24.911Z caller=main.go:310 msg="No time or size retention was set so using the default time retention" duration=15d
[…]
level=info ts=2020-09-25T10:04:24.916Z caller=main.go:673 msg="Server is ready to receive web requests."
```

- 运行在原生的Docker容器里：

```
docker run -p 9090:9090 -v /tmp/prometheus.yml:/etc/prometheus/prometheus.yml \
       prom/prometheus
```

- Kubernetes Deployments / StatefulSets

可以将上面的 Docker 容器配置在 Deployments / StatefulSets 里运行，使用 ConfigMap 来提供配置。

社区维护了一个Helm可以更加方便的安装

- Helm

```shell
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
helm repo update

# Helm 3
helm install [RELEASE_NAME] prometheus-community/prometheus

```

结果：
```
NAME                                                      READY   STATUS    RESTARTS   AGE
prometheus-kube-state-metrics-66cc6888bd-x9llw   1/1     Running   0          93d
prometheus-node-exporter-h2qx5                   1/1     Running   0          10d
prometheus-node-exporter-k6jvh                   1/1     Running   0          10d
prometheus-node-exporter-thtsr                   1/1     Running   0          10d
prometheus-server-0                              2/2     Running   0          90m
```

Helm 部署了 node-exporter, kube-state-metrics, 以及 alertmanager, 所以可以立刻开始监控节点和集群。

- Kubernetes operator

一种更高级的部署方式是使用 [Prometheus operator](https://github.com/prometheus-operator/prometheus-operator)

---

### 如何使用 Prometheus 监控 Kubernetes service

未完待续
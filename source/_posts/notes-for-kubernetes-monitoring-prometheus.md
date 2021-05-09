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

下面的Traefik可以用Ingress-Ingix替代。

Traefik是一个与微服务和Kubernetes紧密集成的反向代理，作为Ingress控制器，是你的微服务和互联网之间的桥梁。

简单的安装Traefik
```shell
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
helm install traefik stable/traefik --set metrics.prometheus.enabled=true
```

结果
```
$ kubectl get svc
k get svc
NAME         TYPE            CLUSTER-IP       EXTERNAL-IP                                                               PORT(S)                     AGE
kubernetes  ClusterIP        100.64.0.1       <none>                                                                    443/TCP                     99d
traefik    LoadBalancer      100.65.9.227 xxx.eu-west-1.elb.amazonaws.com   443:32164/TCP,80:31829/TCP  72m
traefik-prometheus ClusterIP 100.66.30.208    <none>                                                                    9100/TCP                    72m
```

Traefik 已经内置了 Prometheus 的指标：

```shell
$ curl 100.66.30.208:9100/metrics
# HELP go_gc_duration_seconds A summary of the GC invocation durations.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 2.4895e-05
go_gc_duration_seconds{quantile="0.25"} 4.4988e-05
...
```

现在我们为 `prometheus.yml` 增加新的目标，首先看一下目前的配置：
```shell
kubectl get cm prometheus-server -o yaml
```
从结果中可以看到，Prometheus 其实也监控自己：
```yaml
  - job_name: 'prometheus'
    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.
    static_configs:
    - targets: ['localhost:9090']
```

我们增加一个静态的端点：
```shell
kubectl edit cm prometheus-server  
```

```yaml
  - job_name: 'traefik'
    static_configs:
    - targets: ['traefik-prometheus:9100]
```

如果服务不在同一个命名空间里，需要使用FQDN (例如：`traefik-prometheus.[namespace].svc.cluster.local`)

一些配置：
- `basic_auth` 和 `bearer_token`：用于鉴权
- `kubernetes_sd_configs` 或 `consul_sd_configs` : 服务发现
- `scrape_interval`, `scrape_limit`, `scrape_timeout`

现在访问 Promoeheus 服务器的 `/targets`， 可以看到 Traefix 的端点:

![picture](https://478h5m1yrfsa3bbe262u7muv-wpengine.netdna-ssl.com/wp-content/uploads/Blog-Kubernetes-Monitoring-with-Prometheus-5-Traefik-metrics.png)

还可以定位一些 traefix 的指标：

![picture](https://478h5m1yrfsa3bbe262u7muv-wpengine.netdna-ssl.com/wp-content/uploads/Blog-Kubernetes-Monitoring-with-Prometheus-6-Prometheus-query-example.png)

除了使用静态目标外，还可以服务发现：

```yaml
annotations:
  prometheus.io/port: 9216
  prometheus.io/scrape: true
```

---

### 如何使用 Prometheus exporters 监控 Kubernetes 里的服务

虽然一些程序提供了 Prometheus 指标，但是还有很多程序并不提供。这个时候我们需要使用 Prometheus exporters 来做一个“翻译”或者说“适配”。

![picture](https://478h5m1yrfsa3bbe262u7muv-wpengine.netdna-ssl.com/wp-content/uploads/Blog-Kubernetes-Monitoring-with-Prometheus-7-Prometheus-Exporter.png)

这些 exporters 可以和主应用程序位于同一个pod里（以 sidecar 容器的形式），或者位于另外的 pod 里，甚至在不同的基础架构上。

exporters 将主程序的服务指标转换成 Prometheus 指标，并暴露出去，所以你只需要从 exporter 那里拉取数据就可以了。

#### 使用 PromCat：

由于互联网上存在着大量的 Prometheus exporters， Sysdig公司提供了一个网站 PromCat.io，方便大家来查找。

#### 动手实验：使用 Prometheus 监控 Kubernetes 集群里的 Redis 服务

```shell
# kubectl get pod redis-546f6c4c9c-lmf6z
NAME                     READY     STATUS    RESTARTS   AGE
redis-546f6c4c9c-lmf6z   2/2       Running   0          2m
```

安装:
```shell
# Clone the repo if you don't have it already
git clone git@github.com:mateobur/prometheus-monitoring-guide.git
kubectl create -f prometheus-monitoring-guide/redis_prometheus_exporter.yaml
```

配置 exporter:
```yaml
  - job_name: 'redis'
    static_configs:
      - targets: ['redis:9121']
```

这样就得到了Redis的指标：
![picture](https://478h5m1yrfsa3bbe262u7muv-wpengine.netdna-ssl.com/wp-content/uploads/Blog-Kubernetes-Monitoring-with-Prometheus-9-Prometheus-Redis-metrics-example.png)

---

### 使用 Prometheus 和 kube-state-metrics 监控 Kubernetes 集群

除了监控集群里的服务之外，你还可以监控 Kubernetes 集群本身：
- 主机：CPU，内存，磁盘，等等
- 调度流程级别的指标：Deployment 状态, resource 请求, 调度和 api server 延迟, 等等。
- kube-system内部组件：调度器、控制管理器，DNS服务器等的指标

#### Prometheus 用到的 Kubernetes 监控组件：
- cAdvisor: 是一个开源的容器资源使用和性能分析的代理，运行在Kubelet里面，所以可以对外提供节点和Docker的指标。
- Kube-state-metrics：监听Kubernetes API服务器，将对象（如deployments, nodes, pods）的状态转换成指标并对外提供。
- Metrics-server：是一个集群级别的资源使用数据的聚合器。只保留最新的数据，并不负责长期存储。

因此：
- Kube-state-metrics 聚焦于调度流程级别的指标，如 deployment, pod, replica 等等的状态。
- Metrics-server 聚焦于实现  resource metrics API ：CPU，文件描述符，内存，请求演出等等。

#### 使用 Prometheus 监控 Kubernetes 节点：

node-exporter 是 Prometheus 官方提供的监控节点的工具。

使用 Helm 安装:
```shell
# add repo only needed if it wasn't done before
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
# Helm 3
helm install [RELEASE_NAME] prometheus-community/prometheus-node-exporter
```

结果：
```shell
TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                                     AGE
node-exporter-prometheus-node-exporter   ClusterIP   10.101.57.207    <none>        9100/TCP                                    17m
```

如果使用 Helm 安装 Prometheus ，则不需要做任何配置，立刻开始收集和显示节点的指标：

![picture](https://478h5m1yrfsa3bbe262u7muv-wpengine.netdna-ssl.com/wp-content/uploads/Blog-Kubernetes-Monitoring-with-Prometheus-10-Prometheus-node-metrics-example.png)

#### 使用 Prometheus 监控 kube-state-metrics：

如果使用 Helm 安装 Prometheus ，则kube-state-metrics已经安装好。

如果没有的话，这样安装：
```shell
git clone https://github.com/kubernetes/kube-state-metrics.git
kubectl apply -f examples/standard
...
# kubectl get svc -n kube-system
NAME                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)             AGE
kube-dns             ClusterIP   10.96.0.10       <none>        53/UDP,53/TCP       13h
kube-state-metrics   ClusterIP   10.102.12.190    <none>        8080/TCP,8081/TCP   1h
```

配置:
```yaml
  - job_name: 'kube-state-metrics'
    static_configs:
    - targets: ['kube-state-metrics.kube-system.svc.cluster.local:8080']
```

#### 使用 Prometheus 监控 Kubernetes 控制面：

![picture](https://478h5m1yrfsa3bbe262u7muv-wpengine.netdna-ssl.com/wp-content/uploads/Blog-Kubernetes-Monitoring-with-Prometheus-11-Monitoring-Kubernetes-Control-Plane-with-Prometheus.png)

一些 Kubernetes 组件使用 Prometheus 暴露其内部的性能指标：

- [Kubernetes apiserver](https://sysdig.com/blog/monitor-kubernetes-api-server/)
- [kubelet](https://sysdig.com/blog/how-to-monitor-kubelet/)
- [etcd](https://sysdig.com/blog/monitor-etcd/)
- [controller-manager](https://sysdig.com/blog/how-to-monitor-kube-controller-manager/)
- [kube-proxy](https://sysdig.com/blog/monitor-kube-proxy/)
- [kube-dns](https://sysdig.com/blog/how-to-monitor-coredns/)

监控这些组件与监控其他的 Prometheus 端点没什么不同，但有两点需要注意：
- 这些组件很多都只能在 localhost上 侦听，使得它们很难被 Prometheus pod 访问。
- 这些组件可能没有指向 pod 的 Kubernetes Service， 但是你总是可以创建出来。 

下面用Minikube来演示如何监听kube-scheluder:
首先安装二进制文件，然后创建一个在所有接口上对外暴露kube-scheduler服务的集群：

```shell
minikube start --memory=4096 --bootstrapper=kubeadm --extra-config=kubelet.authentication-token-webhook=true --extra-config=kubelet.authorization-mode=Webhook --extra-config=scheduler.address=0.0.0.0 --extra-config=controller-manager.address=0.0.0.0
```

接下来，创建一个指向 kube-scheduler pod 的 service：
```yaml
kind: Service
apiVersion: v1
metadata:
  name: scheduler-service
  namespace: kube-system
spec:
  selector:
    component: kube-scheduler
  ports:
  - name: scheduler
    protocol: TCP
    port: 10251
    targetPort: 10251
```

现在，你可以抓取这个端点：`scheduler-service.kube-system.svc.cluster.local:10251`

---
### 大规模环境下的 Prometheus

[在大规模环境下使用 Prometheus 的挑战](https://sysdig.com/blog/challenges-scale-prometheus/)，一些开源工具，例如 Cortex, Thanos 可以来解决一些问题并增加新功能。

--- 
### 接下来

[一些典型的与Prometheus一起部署的组件](https://sysdig.com/blog/kubernetes-monitoring-with-prometheus-alertmanager-grafana-pushgateway-part-2/)

使用 PromQL 来聚合指标，发出告警，生成可视化仪表板。

[使用 Prometheus operator 和 CRD](https://sysdig.com/blog/kubernetes-monitoring-prometheus-operator-part3/) 

## 本文参考的英文原文：

[https://sysdig.com/blog/kubernetes-monitoring-prometheus/](https://sysdig.com/blog/kubernetes-monitoring-prometheus/)

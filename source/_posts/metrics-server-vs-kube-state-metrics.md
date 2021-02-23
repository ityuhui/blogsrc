title: metrics-server vs kube-state-metrics

date: 2021-01-25 11:36:02

categories:
- kubernetes

tags:
- metrics-server
- kube-state-metrics
---

## metrics-server
Metrics Server collects resource metrics from Kubelets and exposes them in Kubernetes apiserver through Metrics API for use by Horizontal Pod Autoscaler and Vertical Pod Autoscaler. Metrics API can also be accessed by kubectl top, making it easier to debug autoscaling pipelines.

<!-- more -->

Metrics Server is not meant for non-autoscaling purposes. For example, don't use it to forward metrics to monitoring solutions, or as a source of monitoring solution metrics.

## kube-state-metrics
kube-state-metrics is a simple service that listens to the Kubernetes API server and generates metrics about the state of the objects.

It is used for monitor e.g. Prometheus

## kube-state-metrics vs. metrics-server
The metrics-server is a project that has been inspired by Heapster and is implemented to serve the goals of core metrics pipelines in Kubernetes monitoring architecture. It is a cluster level component which periodically scrapes metrics from all Kubernetes nodes served by Kubelet through Summary API. The metrics are aggregated, stored in memory and served in Metrics API format. The metrics-server stores the latest values only and is not responsible for forwarding metrics to third-party destinations.

kube-state-metrics is focused on generating completely new metrics from Kubernetes' object state (e.g. metrics based on deployments, replica sets, etc.). It holds an entire snapshot of Kubernetes state in memory and continuously generates new metrics based off of it. And just like the metrics-server it too is not responsible for exporting its metrics anywhere.

Having kube-state-metrics as a separate project also enables access to these metrics from monitoring systems such as Prometheus.
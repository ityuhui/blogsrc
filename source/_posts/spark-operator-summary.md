title: Spark on Kubernetes Operator

date: 2020-11-29 11:12:58

categories:
- 大数据和分布式计算

tags:
- operator
- spark
- k8s
- kubernetes

---

## 介绍

Spark on k8s operator由GCP出品，用于将Apache Spark部署在kubernetes集群上并使用operator的方式来管理spark应用程序。

<!-- more -->

## 安装

用于可能出现的兼容性的问题，推荐在较新的kubernetes版本上部署。

## 构建新的image用于测试

```shell
docker build -t <image-tag> .
```
根据需要添加代理

替换deployment里的镜像

```shell
kubectl patch deployment sparkoperator-1606724880 --patch '{"spec": {"template": {"spec": {"containers": [{"name": "sparkoperator","image":"tag"}]}}}}' -n "spark-operator"
```

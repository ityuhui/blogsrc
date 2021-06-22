title: etcd 实验

date: 2021-06-22 17:49:23

categories:
- 数据库

tags:
- database
- etcd
---

## Download and Installation

从官方网站下载pre build package

解开压缩包

## Run

```shell
./etcd

ETCDCTL_API=3 ./etcdctl put mykey "this is awesome"

ETCDCTL_API=3 ./etcdctl get mykey
```

<!--more-->

## 说明

默认的端口:

- 2379 For client request

- 2380 For peer communicate

## 组建多机cluster

### 需要解决的主要问题是机器发现（Discovery）

#### Static

在启动参数里指定所有的机器

For example:
cluster name:
etcd_cluster_1

nodes:
yhxa5 9.111.255.120
yhvm1 9.111.255.51

```shell
./etcd --name yhxa5 --initial-advertise-peer-urls http://9.111.255.120:2380 --listen-peer-urls http://9.111.255.120:2380 --listen-client-urls http://9.111.255.120:2379,http://127.0.0.1:2379 --advertise-client-urls http://9.111.255.120:2379 --initial-cluster-token etcd-cluster-1 --initial-cluster yhxa5=http://9.111.255.120:2380,yhvm1=http://9.111.255.51:2380 --initial-cluster-state new

./etcd --name yhvm1 --initial-advertise-peer-urls http://9.111.255.51:2380 --listen-peer-urls http://9.111.255.51:2380 --listen-client-urls http://9.111.255.51:2379,http://127.0.0.1:2379 --advertise-client-urls http://9.111.255.51:2379 --initial-cluster-token etcd-cluster-1 --initial-cluster yhxa5=http://9.111.255.120:2380,yhvm1=http://9.111.255.51:2380 --initial-cluster-state new
```

#### etcd Discovery


#### DNS Discovery




 


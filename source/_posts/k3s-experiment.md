title: K3S 实验总结

date: 2020-11-13 22:21:15

categories:
- kubernetes

tags:
- k3s
---

## 介绍

k3s是rancher公司发布的kubernetes分发版，用于物联网、边缘计算，所以对kubernetes进行了很多剪裁。

<!--more-->

## 导入镜像

在具有docker的机器上

```shell
docker save -o busybox.tar busyboxaaa
scp busybox.tar ubuntu@k3s_machine:~/
```

在k3s_machine上
```shell
ctr image import busybox.tar
```

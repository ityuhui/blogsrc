title: Container Terminology Summary

date: 2021-01-23 17:07:36

categories:
- kubernetes

tags:
- container
- docker
---

## 1 Introduction

There are many terminologies in the domain of container.

<!--more-->

## 2 Terminology

### Open Container Initiative (OCI)
The Open Container Initiative is an open governance structure for the express purpose of creating open industry standards around container formats and runtimes.

### Container Runtime Interface (CRI)
A plugin interface which enables kubelet to use a wide variety of container runtimes.

### docker/moby
A collaborative project for the container ecosystem to assemble container-based systems.

### podman
Another implementation of container manager, the same as docker.

### containerd
An open and reliable container runtime.

### cri-o
Open Container Initiative-based implementation of Kubernetes Container Runtime Interface, the same with containerd, but only used for Kubernetes.

### runc
CLI tool for spawning and running containers according to the OCI specification

### crun
A fast and lightweight fully featured OCI runtime and C library for running containers, the same with runc.

### buildah
A tool that facilitates building OCI images

### skopeo
Work with remote images registries - retrieving information, images, signing content

### crictl
CLI for kubernetes CRI

### ctr
CLI for containerd
## 3 Mainstream scenarios in Kubernetes 

### 3.1
```
kubelet  <--> containerd (with cri-containerd) <--> runc
```
### 3.2
```
kubelet  <--> ori-o <--> runc
```


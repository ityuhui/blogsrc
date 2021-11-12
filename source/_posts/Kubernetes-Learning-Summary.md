title: Kubernetes 学习笔记

date: 2021-01-02 16:05:52

categories:
- kubernetes

tags:
- k8s
- kubernetes
---

## 介绍
Kubernates 是 云计算基础架构，docker是其最基本的运行元素，是一个与EGO高度类似的系统，可以实现：

* 自动扩容
* 失败容器的重新启动 
* 对外服务地址不变

<!-- more -->

## 安装

### 使用kubeadmin安装
- 参考官网的文章
  * [Install the kubeadm setup tool](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
  * [Creating a single control-plane cluster with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)
- 由于一些系统进程所在的docker镜像被GFW阻隔，所以必须使用技术手段访问
- 需要配置docker pull代理
- 需要对Google的docker image托管网站k8s.gcr.io在外网的机器上做nslookup，把查到的地址写到本地的/etc/hosts文件里

#### 添加compute node
- 在compute node上和master上的/etc/hosts里，添加好记录使其可以互相访问
- compute node需要安装好kubeadmin工具
- 安装好master之后，会出现一个用于添加compute node的命令，在compute node上执行这个命令就可以了。但是命令里的Token在24小时之后会过期，所以需要重新获得token, 参考上面的Creating a single control-plane cluster with kubeadm

## 笔记

### 得到yaml定义文件的解释
```
kubectl explain pod.spec
```
### pod

#### 为什么Kubernetes要以pod为基本单元
container的推荐用法是：只运行一个进程和它的子进程
一个业务需要多个相互联系的进程，也就有了多个container, 用pod（本意是豌豆荚）来组织在一起。

#### pod修改了docker默认配置
* 使得在一个pod里的所有container都共享同一个Network和UTS namespace(Linux)
* 所有的container都有一样的hostname和网络接口（一样的IP地址和相同的port空间）
* 但是文件系统不是共享的，必须通过kubernetes提供的volumn来实现

#### 在pod里执行命令
```
kubectl exec downward env
```
#### 获得pod log
```
kubectl logs ${pod_name}
kubectl logs ${pod_name} -c ${container_name}
```
#### 访问 pod
```
$ kubectl port-forward kubia-manual 8888:8080
... Forwarding from 127.0.0.1:8888 -> 8080
... Forwarding from [::1]:8888 -> 8080
```

#### 给node加上label 限制pod调度
```
apiVersion: v1
kind: Pod
metadata:
  name: kubia-gpu
spec:  
  nodeSelector:               
    gpu: "true"               
  containers:  
    - image: luksa/kubia
      name: kubia
```

#### 从pod内部到宿主机拷贝文件

  1. `kubectl cp /主机目录/文件路径 podName:/容器路径/xxx.datasource -n namespaces`
这样可以把主机目录文件拷贝到容器内

  2. `kubectl cp podName:容器路径/xxx.datasource -n namespaces /主机目录`
这样可以把容器内文件cp到主机目录

*注意：从容器拷贝文件到主机的时候podNname:这里不要加/ 之前加了/会一直报错

#### pod 分组
##### tag
##### namespace

#### 获得pod的动态信息

DOWNWARD API

#### 向pod传入配置

  - ConfigMap
  - Secret

### Volumes
Volumn不是顶级的资源，而是pod的一部分，生命周期和pod一致。

#### emptyDir
pod启动的时候是空目录，pod结束的时候就会被删除，用于pod内各个container共享文件

- 使用内存(tmpfs)
```yaml
volumes:  
  - name: html    
    emptyDir:      
      medium: Memory
```

#### hostPath

#### local

#### gitRepo

#### nfs

#### gcePersistentDisk

#### cinder, cephfs...

#### configMap, secret, downwardAPI

#### PVC

## Service
其实是网络代理，用于外部访问内部，内部访问外部，内部互访

## Port forward
```bash
kubectl port-forward fortune 8080:80
```
可以用于快速的暴露内部端口，产品级使用要使用Service

### 支持短任务Job

#### Job
短任务

#### CronJob 
定期任务

### 访问Pod:
    {pod-ip}.{namespace}.pod.cluster.local //例如某pod的ip为  1.2.3.4,在命名空间default与DNS名称cluster.local将有一个域名：1-2-3-4.default.pod.cluster.local。

    {hostname}.{subdomain}.{namespace}.svc.cluster.local
    subdomain是在创建pod设定的属性,和hostname必须一起设置，同时必须设置headless service ( name == pod subdomain )
    
    访问StatefulSet:
    {pod-name}.{service-name}.{namespace}.svc.cluster.local
    可以进入到pod中查看/etc/hosts
    
    访问Service:
    {service-name}.{namespace}.svc.cluster.local
    
### 批量删除

```bash
kubectl get pods | grep pod-ego-activity | awk '{print $1}' | xargs kubectl delete pod

kubectl get activity | grep activity | awk '{print $1}' | xargs kubectl delete activity
```

### kuberadmin生成的证书有效期是1年，到期前必须更新

```shell
kubeadm alpha certs check-expiration

kubeadm alpha certs renew all
cp /etc/kubernetes/admin.conf ~/.kube/config 
```

### 查看kubelet的错误

```shell
journalctl -xefu kubelet 
```

### 更改deployment里的image
```shell
kubectl patch deployment sparkoperator-1606724880 --patch '{"spec": {"template": {"spec": {"containers": [{"name": "sparkoperator","image":"yh-spark-operator:v201201.1"}]}}}}' -n "spark-operator"
```

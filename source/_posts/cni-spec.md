title: 容器网络接口（CNI）规范

date: 2022-07-21 12:13:36

categories:
- kubernetes

tags:
- container
- docker
- cni
- flannel

---

## 配置

网络配置格式，stdin 输入给 CNI plugin，或者保存为配置文件，具体内容如下：

<!--more-->

```yaml
{
  "cniVersion": "1.0.0",
  "name": "dbnet",
  "plugins": [
    {
      "type": "bridge",
      // plugin specific parameters
      "bridge": "cni0",
      "keyA": ["some more", "plugin specific", "configuration"],
      
      "ipam": {
        "type": "host-local",
        // ipam specific
        "subnet": "10.1.0.0/16",
        "gateway": "10.1.0.1",
        "routes": [
            {"dst": "0.0.0.0/0"}
        ]
      },
      "dns": {
        "nameservers": [ "10.1.0.1" ]
      }
    },
    {
      "type": "tuning",
      "capabilities": {
        "mac": true
      },
      "sysctl": {
        "net.core.somaxconn": "500"
      }
    },
    {
        "type": "portmap",
        "capabilities": {"portMappings": true}
    }
  ]
}
```

## 执行协议

### 参数

- CNI_COMMAND
- CNI_CONTAINERID
- CNI_NETNS
- CNI_IFNAME
- CNI_ARGS
- CNI_PATH

### 操作

- ADD
- DEL
- CHECK
- VERSION

## 网络配置执行

- Adding an attachment
- Deleting an attachment
- Checking an attachment
- Deriving execution configuration from plugin configuration

## 插件代理

`ipam`

## 结果类型

## 总结

当容器要创建或者删除网络接口的时候，容器运行时(在 K8S 里是 Kubelet，应该不是 containerd )调用容器网络接口运行时，CNI将配置从Stdin喂入，调用插件，给容器设置网络，结束后将结果以json格式输出。

CNI 用于建立 container 之间的网络，不负责物理节点之间的网络连接。

以 Flannel 举例，执行
```bash
kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
```

将会在 Kubernetes 集群里，创建一个 daemonset, 从而在每个节点上，创建一个 deployment, 从而创建出一个 pod, 里面包含两个 container：

一个是 initContainers，image 是 docker.io/rancher/mirrored-flannelcni-flannel-cni-plugin，用处是将 image 里面的 CNI 插件下载到节点本地的 /opt/cni/bin，例如 /opt/cni/bin/flannel

另外一个 container， image 里包含 /opt/bin/flanneld，这个容器运行后，flanneld 会创建出一个文件 `/run/flannel/subnet.env`, 里面的内容是 flanneld 运行的节点的网络信息，例如：

```
FLANNEL_NETWORK=10.1.0.0/16
FLANNEL_SUBNET=10.1.17.1/24
FLANNEL_MTU=1472
FLANNEL_IPMASQ=true
```

当 Kubernetes 需要增加或删除 pod 时，kubelet 会调用 CNI 插件 flannel （配置位于 `/etc/cni/net.d/10-flannel.conflist`)，flannel 读取上面的文件 `subnet.env`，继续配置其他的 CNI 插件（例如 bridge),完成容器网络的配置。

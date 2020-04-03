title: 使用kubebuilder创建kubernetes的operator

date: 2019-10-06 17:04:20

categories:
- kubernetes

tags:
- k8s
- kubernetes
---

## 介绍

在kubernetes（以下简称k8s）里，operator指的是由CRD和controller共同构成的某项业务。CRD负责表示业务数据，controller负责业务操作（对业务数据的修改），两者共同完成某项业务在k8s里的运营。

创建CRD不需要编写程序，只需要写yaml文件，然后使用kubectrl命令部署到k8s里面就可以了，CRD部署到k8s之后，数据是存储在etcd里面的，只能手工（例如使用kubectrl）查询和修改，并没有什么实际作用，要想自动完成实际的业务，需要controller来实现。

<!-- more -->

创建controller需要编程，controller的基本的流程是：

 1. 监听CRD的变化
通过向API server放置watch/informer，当CRD发生变化，API server会通知controller
 2. 操作
根据业务需要，对获得的CRD或者k8s里的其他资源进行修改
 3. 写回
将变更的CRD信息写回API server

其中，第一步和第三步都可以通过REST操作来完成，所以理论上使用任何的编程语言都可以编写controller，但是，k8s社区已经把这些操作都封装成了各种语言的包来调用，省去了我们直接操作REST的不方便（特别是鉴权），在这些语言的包里，最推荐的无疑是k8s的原生语言golang编写的client-go

虽然有了client-go，我们还是需要自己编写很多的与具体业务无关的基础框架代码，例如监听CRD变化，写回状态，以及编写CRD的yaml，为了加快Operator的编写，k8s社区提供了kubebuilder，它可以为我们生成基础框架代码和CRD的yaml，我们只需要填写业务的数据成员和业务代码就可以了。

## 安装

### 安装 kubebuilder

[Reference](https://book.kubebuilder.io/quick-start.html#installation)

### 安装 kustomize

## 使用

### 初始化

```shell
go mod init module_name

kubebuilder init --domain example.com
```

### 创建API和controller

```shell
kubebuilder create api --group ego --version v1 --kind Activity
```

### 运行

```shell
make install # 安装CRD
make run # 启动controller
```

## 部署

```shell
make docker-build docker-push IMG=yuhuixa/manager-controller
make deploy
```

## 开发

### 增加对象数据参数

```go
// ActivitySpec defines the desired state of Activity
type ActivitySpec struct {
        Command string `json:"command"`
        Host    string `json:"host"`
        // +optional
        Execuser string `json:"execuser"`
        // +optional
        Execcwd string `json:"execcwd"`
        // +optional
        Envs []string `json:"envs"`
}

// ActivityStatus defines the observed state of Activity
type ActivityStatus struct {
        ProSta string `json:"prosta"`
}

```

```shell
make && make install && make run
kustomize build config/default # 重新渲染yaml
kubectl apply -f config/samples
```

### 实现接口 Reconcile

```go

import (
        "context"
        "fmt"
        "strings"

        "github.com/go-logr/logr"
        corev1 "k8s.io/api/core/v1"
        metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
        ctrl "sigs.k8s.io/controller-runtime"
        "sigs.k8s.io/controller-runtime/pkg/client"

        egov1 "symoperator/api/v1"
)

func (r *VirtulMachineReconciler) Reconcile(req ctrl.Request) (ctrl.Result, error) {
        ctx := context.Background()
        log := r.Log.WithValues("activity", req.NamespacedName)

        activity := &egov1.Activity{}
        if err := r.Get(ctx, req.NamespacedName, activity); err != nil {
                log.Error(err, "unable to fetch activity")
        } else {
                fmt.Println("activity.Spec.Command: ", activity.Spec.Command)
                fmt.Println("activity.Spec.Host: ", activity.Spec.Host)
                fmt.Println("activity.Status.ProSta: " + activity.Status.ProSta)
        }
}
```

### 一些有用的参考代码

#### 获得系统pod

```go
        podList := &corev1.PodList{}
        err := r.List(ctx, podList, client.InNamespace("kube-system"))
        if err != nil {
                fmt.Printf("failed to list pods in namespace default: %v\n", err)
        } else {
                for _, pod := range podList.Items {
                        fmt.Println(pod.Spec.NodeName)
                }
        }

```

#### 创建一个只运行一次的pod

```go
        podName := "pod-sample-" + strconv.FormatInt(time.Now().Unix(), 10)
        podCmd := []string{"sleep"}
        podArgs := []string{"50"}

        pod := &corev1.Pod{
                ObjectMeta: metav1.ObjectMeta{
                        Namespace: "default",
                        Name:      podName,
                },
                Spec: corev1.PodSpec{
                        Containers: []corev1.Container{
                                corev1.Container{
                                        Image:   "ubuntu",
                                        Name:    "ubuntu",
                                        Command: podCmd,
                                        Args:    podArgs,
                                },
                        },
                        RestartPolicy: "Never",
                },
        }
        // c is a created client.
        _ = r.Create(ctx, pod)

```

## 参考

- [使用kubebuilder开发kubernetes CRD](https://segmentfault.com/a/1190000019892302)
- [K8S resource CURD samples](https://github.com/kubernetes-sigs/controller-runtime/blob/master/pkg/client/example_test.go)

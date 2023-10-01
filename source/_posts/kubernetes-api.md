title: Kubernetes API

date: 2023-10-01 17:37:45

categories:

- kubernetes

tags:

- k8s
- kubernetes
- API
- apimachinery
- controller

---

# Kubernetes API

## OpenAPI Spec

Kubernetes Open API spec 有两个版本，同时存在

2.0

```bash
https://github.com/kubernetes/kubernetes/blob/master/api/openapi-spec/swagger.json
```

3.0

```bash
https://github.com/kubernetes/kubernetes/tree/master/api/openapi-spec
```

目前，各个语言的客户端都用 2.0 生成。

[kubernetes/kube-openapi](https://github.com/kubernetes/kube-openapi/) 项目，扫描 kubernetes 源代码，根据 `// tag` 的指示，生成 spec 文件，我个人认为会同时生成 2.0 和 3.0 的文件。

<!-- more -->

## 代码自动生成

[k8s.io/apimachinery](https://github.com/kubernetes/kubernetes/tree/master/staging/src/k8s.io/apimachinery) 提供 go 语言的结构体 meta 定义，供 [k8s.io/api] 使用

[k8s.io/api](https://github.com/kubernetes/kubernetes/tree/master/staging/src/k8s.io/api) 是 Kubernetes API resource 的 scheme

[kubernetes/code-generator](https://github.com/kubernetes/code-generator) 根据源代码里的 golang 数据结构生成 CRD 的client，包含 CRD 的 CURD 函数，深拷贝，informer，lister

[k8s.io/client-go](https://github.com/kubernetes/kubernetes/tree/master/staging/src/k8s.io/client-go) 用于编写 Kubernetes golang 客户端，包含两种类型的 client：
dynamic (generic，自己输入GVK)
typed （GVK Scheme 已知）

[kubernetes-sigs/controller-runtime](https://github.com/kubernetes-sigs/controller-runtime) 第三个 Kubernetes golang 客户端，用于写Operator控制器，kubebuilder 和 OpenShift Operator SDK 底层使用的就是它

[kubernetes-sigs/controller-tools](https://github.com/kubernetes-sigs/controller-tools) 包含 controller-gen，生成 CRD， RBAC 等

## 写 Operator 的三种方式

### 1 sample-controller

使用 code-generator 和 client-go

### 2 Kubebuilder

使用 controller-runtime 和 controller-tools

### 3 Opeartor SDK

使用 controller-runtime 和 controller-tools

## release Operator

### Helm

参数化一组 yaml 文件

### Kustomize

### 其它

- sed, awk, jq, yq
- Ansible
- Ksonnet 已经废弃

## API Server

[k8s.io/apiserver/pkg/registry/rest](https://github.com/kubernetes/kubernetes/tree/master/staging/src/k8s.io//apiserver/pkg/registry/rest) 定义了 API server 的 REST 接口

[k8s.io/apiserver/pkg/registry/generic](https://github.com/kubernetes/kubernetes/tree/master/staging/src/k8s.io/apiserver/pkg/registry/generic) 在 generic object 上实现了这个接口

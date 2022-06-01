title: Kubernetes OIDC 鉴权

date: 2020-08-06 19:15:14

categories:
- kubernetes

tags:
- Keycloak
- oidc
---

## 介绍

Kubernetes本身不提供用户管理，所以，Keycloak可以做用户管理和客户端管理。

## Keycloak 配置

### 创建客户端

进入管理界面
```
http://localhost:8080
```

进入前一篇文章[Keycloak](https://ityuhui.github.io/2020/08/05/Keycloak-Introduction/)创建的Realm kubernetes界面

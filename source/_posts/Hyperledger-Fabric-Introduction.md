title: Hyperledger Fabric的简单介绍

date: 2019-12-16 21:36:54

categories:
- 区块链和以太坊

tags:
- block chain
- hyperledger

---

## 介绍

Hyperledger是Linux基金会旗下的项目，里面包含了多个区块链的实现以及辅助项目。如Fabric, Indy, Iroha, Sawtooth等。

<!--more-->

## Fabric

Fabric是Hyperledger项目里最早也是目前应用最广泛的区块链项目，最初由IBM开发，后来捐助给基金会。

### 组成
- 客户端应用程序
- 认可节点（chaincode也就是智能合约运行在这上面，认可节点同时也作为提交节点）
- 提交节点
- 排序服务器
- 账本

### 流程
1. 客户端应用程序 向 认可节点 发送事务请求；
2. 认可节点开始认可（模拟运行），并把签名的认可结果发回客户端应用程序；
3. 客户端应用程序然后再提交到 排序服务器上（之前使用kafka，自v1.14.1后改用raft);
4. 排序服务器通知提交节点创建一个block，提交到账本上，完成一次事务。


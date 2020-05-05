title: Microsoft HPC Pack 2016 学习笔记
date: 2020-05-05 16:10:43
categories:
- 大数据和分布式计算
tags:
- distributed computing
- Microsoft
- HPC
---
## 介绍

Microsoft HPC Pack是微软的高性能分布式计算平台，类似于IBM Spectrum Symphony，是一种基于SOA架构的分布式计算框架。目前，它有三种部署方式如下，本文只介绍第一种。

- on-premises, 部署在本地，可以把计算节点扩展到云上
- hybird, 部署在本地，通常会把计算节点扩展到云上
- on-demand 部署在云上

<!--more-->

## on-premise部署

### 1. 部署准备

#### 1.1 评估操作系统是否达到要求

##### 硬件要求

头节点

- CPU: 64位，推荐8核心以上，最小4核心
- 内存：推荐16 GB以上，最小8 GB
- 磁盘：推荐100 GB以上，最小50 GB

其他节点

- CPU: 64位，推荐4核心以上，最小4核心
- 内存：推荐4 GB以上，最小2 GB
- 磁盘：推荐80 GB以上，最小50 GB

##### 软件要求

.NET Framework 4.6.1 (or later)

头节点： Windows Server 2016, Windows Server 2012 R2

计算节点：Windows Server 2019 (only for HPC Pack 2016 Update 3), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1， 

客户端节点：Windows 10, Windows 8.1

Linux node： Red Hat Enterprise Linux 7.0 - 7.6, Red Hat Enterprise Linux 6.7 - 6.10, CentOS-based 7.0 - 7.6, CentOS-based 6.7 - 6.10, Ubuntu Server 14.04 LTS, Ubuntu Server 16.04 LTS, Ubuntu Server 18.04 LTS, SUSE Linux Enterprise Server 12

1.2 评估是否需要High Availability

如果需要，就需要安装三个头节点配置为一个Service Fabric集群

1.3 决定是否需要远程数据库

1.4 决定需要多少个节点

- 计算节点，用于执行任务
- 代理节点(Windows Communication Foundation (WCF) broker nodes)，负责路由SOA服务
- 工作站节点（workstation nodes），可以临时用于执行任务

1.5 选择活动目录域

1.6 选择域账户来添加节点

1.7 为集群选择网络拓扑结构

1.8 准备两个证书用于节点之间的加密通讯

### 2. 部署头节点

2.1 在头节点上安装Windows Server

2.2 将头节点加入活动目录域里

2.3 在前两个头节点上安装前置组件（可选）

这一步是可选的，如果需要配置多个头节点才需要

2.4 在最后一个头节点上安装Microsoft HPC Pack

### 3. 配置集群

3.1 配置集群的网络

3.2 提供安装凭证

3.3 配置新加入的节点的命名规则

3.4 为部署导入或者创建证书

3.5 创建节点模板（可选）

3.6 创建用户（可选）

### 4. 向集群里添加Windows计算节点

4.1 通过模板从物理机（bare metal）上部署节点

4.2 手工向集群添加节点

4.2.1 在计算节点上安装Windows操作系统

4.2.2 将计算节点加入域

4.2.3 在计算节点上安装Microsoft HPC Pack 2016

### 5. 向集群里添加Linux计算节点

5.1 在计算节点上安装Linux操作系统

5.2 下载Linux计算节点安装文件
在Windows头节点上执行Powershell命名

```powershell
Add-PSSnapin microsoft.hpc 

Get-HpcClusterRegistry -PropertyName InstallShare
```

5.3 搭建文件共享路径将5.2下载得到的文件共享给Linux计算节点

5.4 安装证书用于加密HPC节点之间的通讯

5.5 在Linux计算节点上安装Linux计算节点代理

```bash
python setup.py -install -connectionstring:'<connection string of the cluster>' -certfile:'<path to PFX certificate>'
```

## HPC Job Manager

使用HPC Job Manager，可以提交、监控和管理所有的计算任务。

基本的术语：

- Job, 一次计算任务

- Task, 一个Job包含一个或者多个Task, Task不能脱离Job

- Queue, Job提交以后会放在Queue里面，等待调度和分配到计算节点上

- HPC Job Scheduler Service, 运行在头节点上的一个服务，负责调度队列里面的Job/Task，分配资源、分发任务到计算节点、监控任务的执行过程。

## 教程

### Excel 2016 offloading to Azure cluster

这个教程展示了将Excel 2016 放在Azure集群上运行

#### 前提

已经在本地的计算节点上安装好了Excel 2016和HPC Pack 2016 client utilities

#### 步骤

##### 1. 在Azure上部署一个Excel集群， 设置头节点不要参加计算，因为头节点并没有安装Excel

##### 2. 激活Execl产品，你必须要有一个Office的License

##### 3. 使用Execl workbook offloading

3.1 下载sample [xlsb](https://github.com/amat27/HPC2016.SampleCode/raw/master/Excel/AzureSamplePack/Example2/ConvertiblePricing_Complete.xlsb) 

3.2 在Excel 2016里将ConvertiblePricing_Complete.xlsb打开，并激活Excel Options -> Customize Ribbon

3.3 在Develop ribbon，点击COM Add-Ins, 确认HPC Pack Excel COM Add-in已经成功载入

3.4 编辑Excel文件里的VBA宏HPCControlMacros 

```VB
'change Private Const HPC_ClusterScheduler = "hpchn01laj2kdgetycrw.southeastasia.cloudapp.azure.com" to
Private Const HPC_ClusterScheduler = "<headnode DNS name saved above>"
'change Private Const HPC_DependFiles = "D:\tmp\iaasexcel\upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb" to
Private Const HPC_DependFiles = "<upload directory path>\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"
'change HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\hpcadmin", Password:="********" to
HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="<domain>\<username>", Password:="<YourPassword>"
```

3.5 将Excel workbook拷贝到上面指定的HPC_DependsFiles目录里

3.6 调集worksheet里的Cluster按钮，workbook将在会Azure里计算

## 参考

[Overview of Microsoft HPC Pack 2016](https://docs.microsoft.com/en-us/powershell/high-performance-computing/overview?view=hpc16-ps)

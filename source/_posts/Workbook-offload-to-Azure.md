title: 在Azure云上使用HPC Services for Excel运行Excel运算

date: 2020-06-13 21:29:22

categories:
- 大数据和分布式计算

tags:
- bigdata
- distributed computing
- 分布式计算
- 大数据
- HPC
---

## 前提条件

你需要安装最低Windows HPC server 2012 SP1

在你的桌面机（你用来做Excel运算的机器）上，你需要安装Excel 2010和HPC client utilities.

你还需要部署一些Azure虚拟机节点，安装有Excel,用于实际运算。

<!-- more -->

## 例子

### ExcelService配置

在运行ExcelService之前，我们需要现在ZzureNode上部署excel service

1. 打包依赖文件

2. 将这些文件上传到云存储里

3. 同步到Azure节点上

### 例子1：在云上使用一个静态的workbook

#### 步骤

1.创建包

```Powershell
> hpcpack create ConvertiblePricing_AzureCloud_Static.zip ConvertiblePricing_AzureCloud_Static.xlsb
```

2.上传包

```Powershell
> hpcpack upload ConvertiblePricing_AzureCloud_Static.zip /scheduler:HEADNODE /nodetemplate:"Default AzureNode Template"
```

3.同步到Azure节点上

```Powershell
> clusrun /scheduler:HEADNODE /template:AzureTemplate hpcsync
```

4.配置

打开Excel文件，Alt+F11打开宏，修改HPCControlMacros

```VB
Private Const HPC_ClusterScheduler = "HEADNODE"
```

5.运行

先使用Calculate on Desktop测试在本机上运行

再使用Calculate on Cloud来测试在云上运行，你会发现这次快很多，因为每一个单元格的计算都会发送给云的计算节点单独运算

### 例子2：在云上使用一个动态的workbook

与第一个例子不同，这个例子实现并没有向云上的计算节点部署Excel文件，而是在运行过程中通过一个帮助程序来下载Excel

### 例子3：使用SOA服务的Excel和Azure

上面两个例子都是使用Excel VBA来直接运行计算，我们还可以定制一个SOA Service，在这个Service里面，使用HPC/Excel库来做计算。我们还可以做一个定制的客户端，运行在桌面机上，使用云端的服务。这个Case和IBM Spectrum Symphony SOAM application已经基本一致了。

#### 安装包

##### 编译SOA service和client

##### 安装SOA service

#### 运行示例代码
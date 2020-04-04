title: Excel Workbook Offloading to IBM Spectrum Symphony

date: 2020-04-04 14:29:22

categories:
- 大数据和分布式计算

tags:
- bigdata
- distributed computing
- 分布式计算
- 大数据
- Symphony
---

将Excel的计算分发到IBM Spectrum Symphony集群上进行

## 背景

IBM Spectrum Symphony是基于SOA架构的分布式计算框架，可以将任务调度到集群上计算并汇总计算结果。与之类似的框架还有Apache Hadoop, Apache Spark, Microsoft HPC Pack。受益于其底层优秀的资源调度框架EGO、由C++实现的中间件，Symphony的性能和可扩展性都极为优秀，在金融衍生品的定价以及风险模拟等金融领域得到广泛的应用。

很多数据分析师喜欢使用Microsoft Excel来进行数据的收集、建模和分析，但是，当金融模型的数据量很大，或者模型的计算非常复杂时，在单机上执行Excel数学运算将会极其耗费时间，所以，将Exel workbook上的数据分发到集群进行分布式计算非常必要。IBM Spectrum Symphony支持这一应用场景。

<!-- more -->

## 实现方式

### 1. Execl VBA模式

将Excel作为客户端，当点击workbook/sheet上的宏计算按钮之后，Excel通过IBM Spectrum Symphony COM SDK连接Symphony集群，集群把计算任务和workbook分发到计算节点上，由服务端程序打开Excel workbook进行计算，计算结果返回给客户机器的Excel后，Excel将其填入单元格内。

这种模式下，服务端也可能是由编程语言自制的程序，Excel客户端只发来数据，不发来workbook, 服务端将发来的数据进行处理，返回给Excel客户端。

### 2. 定制客户端和服务端程序的模式

使用任意一种编程语言编写自制的客户端程序，将参数和Excel workbook通过IBM Spectrum Symphony SDK发送给Symphony集群上自制的服务端程序，由服务端程序进行计算，计算结果返回给客户端，客户端再将结果进行后续的处理。

这种模式下，服务端也可根据需要，打开计算节点上的Excel对发送过来的workbook进行处理，将结果返回给客户端。

其实，如果把客户端实现为基于IBM Spectrum Symphony COM SDK的Excel workbook和宏, 把服务端实现为打开Excel workbook执行宏，这种模式就成为了第一种，所以我们可以认为，第一种模式是第二种模式的一个特例。接下来，我会展示一个第一种模式的实例来详细介绍一下。

## Excel VBA模式 实例

### 1. 编写服务端和客户端程序

#### a) 编写Excel客户端

首先使用Excel新建一个workbook, Alt+F11进入Visual Basic for Application界面。

点击Tools->Reference，Browse, 找到并选中IBM Spectrum Symphony COM SDK的DLL文件。

然后，新建类模块MyMessage，用于在客户端和服务端传送消息。

第三，新建一个宏SymphonyClient, 用于放置客户端VBA代码，下面是一个基本的框架：

```VB
'' 初始化

'' 建立连接

'' 发送计算

'' 接收计算结果

'' 在单元格内显示
```

第四，在Excel workbook的界面上，新增一个按钮，将按钮的响应函数指向上面一步写好的函数上

#### b) 编写服务端

打开上面创建的workbook, 新建一个宏SymphonyService, 用于放置服务端VBA代码

```VB
'' 实际的计算逻辑代码
```

使用Symphony C++ SDK，编写打开Excel workbook并执行宏的代码，用于启动Excel计算

### 2. 打包和部署服务端

在Symphony Web管理界面上，找到或者新建一个拥有合适的resource group的consumer，在resource plan界面下，选择好slot分配。

在服务端的Application Profile文件里，填入正确的consumer。

将服务端打包，使用soamdeploy部署到Repository Server（RS）上，使用soamreg命令注册服务端程序，再通过soamview查看服务端程序是否已经激活。

### 3. 运行客户端

在Excel workbook上，点击上面新建好的按钮，计算将会发送给Symphony集群，等计算完成后数据会从集群发送回来，Excel workbook收到后更新单元格上的数据。

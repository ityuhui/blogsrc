title: Windows平台下进程的资源限制(Job Object)

date: 2020-04-08 19:58:56

categories:
- Windows编程

tags:
- Windows
---
## 介绍

与Linux平台上的cgroups类似， Windows平台上也有限制进程资源使用的机制，叫做Job Object，这篇文章是我对微软官方文档的中文翻译，然后加上我自己写的示例代码，代码和官方文档的链接都在文末。

<!--more-->

## Job Object

一个job object将一组进程管理成一个单元，它是可命名的、安全的、可共享的控制进程属性的对象。对一个job object的操作将会影响它管理的所有的进程，例如，可以通过修改job object来影响其管理的所有进程的working set的大小、优先级以及终止所有进程。

### 创建Jobs

函数CreateJobObject用来创建一个job object。新创建出来的job object没有关联任何进程。

函数AssignProcessToJobObject可以将一个进程关联到一个job上，当进程被关联到job上之后，就无法再分开。但是一个进程可以被关联到多个嵌套的job上。

嵌套job是从Windows 8和Windows Server 2012才引入的，所以在之前的操作系统里，一个进程只能被关联到一个job object上，而且一旦关联就无法再分开。

当调用CreateJobObject创建job object的时候，可以给job object指定security descriptor。

### 管理Job关联的进程

当一个进程关联到一个job之后，这个进程创建的子进程默认也会被关联到这个job上。（注意：CreateProcess函数创建的子进程会被自动关联，但是Win32_Process.Create创建的则不会。）

可以通过设置JOB_OBJECT_LIMIT_BREAKAWAY_OK或者JOB_OBJECT_LIMIT_SILENT_BREAKAWAY_OK来修改默认行为：

- 如果job有extended limit/JOB_OBJECT_LIMIT_BREAKAWAY_OK, 并且在创建父进程的时候指定了CREATE_BREAKAWAY_FROM_JOB，那么子进程不会被自动的关联到父进程的job object。

- 如果job有extended limit/JOB_OBJECT_LIMIT_SILENT_BREAKAWAY_OK，不需要在创建父进程的时候指定任何选项，子进程都不会自动被关联到父进程的job object。

如果job是嵌套的，那么层级里的父job的breakaway设置会影响到层级里的其他job所关联的子进程。

函数IsProcessInJob可以判定一个进程是否运行在一个job里。

函数TerminateJobObject可以终止一个job里关联的所有的进程的运行。

### Job限制和通知

job可以强制设置它所关联的每一个进程的working set大小、进程优先级以及执行时间等限制。如果job所关联的进程试图超过限制，有两种结果（默认是第一种）：

1. 进程申请资源表面上返回成功，其实并没有被处理。
2. 允许进程使用超过限制的资源，但是会触发一个通知。

函数SetInformationJobObject用于设置job的限制。以下是资源限制的种类：

- JOBOBJECT_BASIC_LIMIT_INFORMATION
- JOBOBJECT_BASIC_UI_RESTRICTIONS
- JOBOBJECT_CPU_RATE_CONTROL_INFORMATION
- JOBOBJECT_EXTENDED_LIMIT_INFORMATION
- JOBOBJECT_NOTIFICATION_LIMIT_INFORMATION

如果job是嵌套的，层级里的父job会影响子job

如果job有一个关联的I/O completion端口，它可以在资源超限后收到通知。当资源超限或者某个事件来到，系统会发送消息给completion端口。使用带有job object信息类JobObjectAssociateCompletionPortInformation和一个JOBOBJECT_ASSOCIATE_COMPLETION_PORT结构体指针的函数SetInformationJobObject可以将一个completion端口关联到job。注意最好是在job不活动的时候做这个关联，以降低丢失消息的风险。

如果job调用了PostQueuedCompletionStatus函数，所有的消息都会被job直接发送。某个线程必须使用GetQueuedCompletionStatus函数来监控complition端口从而拿到消息。

带有JobObjectNotificationLimitInformation信息类的限制的异常，并不能保证被发送给completion端口，带有JobObjectNotificationLimitInformationx的通知是可以保证的。

### Job的资源账户

job object记录了其关联的所有进程的基本资源信息（包括终止的进程），使用QueryInformationJobObject函数可以获取这些资源信息。

- JOBOBJECT_BASIC_ACCOUNTING_INFORMATION
- JOBOBJECT_BASIC_AND_IO_ACCOUNTING_INFORMATION

如果一个job object是嵌套的，每一个子job的资源账户都会被累加到它的父job的资源账户上。

### 管理Job Object本身

因指定的end-of-job时间限制到达，造成一个job object关联的所有进程都终止时，job object的状态会被设置为signaled。我们可以使用 WaitForSingleObject或者WaitForSingleObjectEx来监控job object来获得这个信号。

指定job object名称、使用OpenJobObject函数可以获得一个已存在的job object的handle。

使用CloseHandle函数可以管理一个job object handle。当一个job所关联的所有的进程都终止并且job的最后一个handle被关闭，这个job就将被销毁。但是，如果一个job带有JOB_OBJECT_LIMIT_KILL_ON_JOB_CLOSE标志，那么关闭job的最后一个handle，会强制终止它关联的所有进程并销毁job。如果一个嵌套的job带有JOB_OBJECT_LIMIT_KILL_ON_JOB_CLOSE标志，那么关闭这个job的handle会终止它以及它的子job的所有的进程。

### 使用Job Objects 来管理进程树

从Windows 8和Windows Server 2012起，一个应用程序可以使用嵌套jobs来管理进程树。

但是之前的系统可以使用其他的方法来管理进程树。这里就不作介绍了。有需要可以看文末的参考。

## 嵌套Job

### 嵌套job的层级

在嵌套job里，每一个子job都包含了父job的进程的子集。如果一个已经在某个job里的进程被加到另外一个job里，如果这些job可以形成一个有效的层级并且没有任何一个job设置了UI限制，那么这些job就成为嵌套job

![图一](https://docs.microsoft.com/en-us/windows/win32/procthread/images/nested-jobs-a.png)

上图展示了一个包含七个进程的进程树的job层级。Job1是job2和job4的父job，它是job3的祖先。job2是job3的父亲。job3是进程P2,P3,P4的直接job

嵌套job也可以用于管理同级的兄弟进程，例如下图，Job1是Job2的父亲。job层级可能只包含进程树的一部分，例如，P0并不在job层级里。

![图二](https://docs.microsoft.com/en-us/windows/win32/procthread/images/nested-jobs-b.png)

### 创建一个嵌套job层级

job层级里的进程可以用AssignProcessToJobObject函数显式的关联到job上，也可以在进程创建的时候自动的关联。job被创建以及进程被关联的顺序决定了层级是否可以被创建出来。

#### 1. 显式关联

所有的job object必须使用CreateJobObject创建，然后多次调用AssignProcessToJobObject，将每一个进程关联到每一个job上，为了确保层级有效，必须首先指定所有的进程到层级的根job上，然后指定进程的子集到直接的子job object上，以此类推。如果按照此顺序指定job,一个子job总是包含父job的进程的子集。如果顺序是随机的话，创建嵌套job将无法成功，AssignProcessToJobObject会返回失败。

#### 2. 隐式关联

当子进程创建的时候，会自动的关联到它的父进程的job链上的所有的job上。直接job object允许脱离（breakaway），子进程脱离直接job object和job链上的每一个job object, 直到遇到了不允许脱离的job object。如果直接job object不允许脱离，那么即使job链上的父job允许脱离，该进程也不能再脱离。

### 嵌套Job里的限制和通知

#### 限制

设置在父job上的限制，会强制应用到子job上。子job的生效的限制值要比父job严格。举例来说，

如果一个子job的优先级类是ABOVE_NORMAL_PRIORITY_CLASS，而父job的优先级类是NORMAL_PRIORITY_CLASS，那么子job的生效的优先级是NORMAL_PRIORITY_CLASS。

但是，如果子job的优先级类是BELOW_NORMAL_PRIORITY_CLASS，那么生效的优先级类是BELOW_NORMAL_PRIORITY_CLASS

下列几种限制都由生效值的问题：

- priority class
- affinity
- commit charge
- per-process execution time limit
- scheduling class limit
- working set minimum and maximum

#### 通知

当特点的事件（例如创建新进程、资源限制越界）发生，一个消息会被发送到job关联的I/O completion端口。job也可以接收消息。对于一个非嵌套的job，消息会被发送到该job关联的completion端口。对于一个嵌套的job,消息会被发送到该job所在的job链上的每一个job关联的completion端口。所以说子job不必一定有自己的completion端口。

### 嵌套Job的资源账户

嵌套job的资源账户信息描述了该job关联的每一个进程的资源使用情况，包括子job的。job链上的每一个job都聚合了它自己关联的进程以及它所在的job链上的子job所关联的进程。

### 终止嵌套Job

当嵌套job里的一个job终止，系统将会终止这个job以及它的子job关联的所有进程。终止的进程的资源将会被父job来计入。

与普通job一样，嵌套job也必须有JOB_OBJECT_TERMINATE访问权限。

## Job Object安全和访问权限

可以控制对Job jobect的访问

当使用CreateJobObject创建job的时候，可以指定一个security descriptor，如果没有指定，job object会有一个默认的security descriptor。默认security descriptor的访问控制列表ACL来自于创建者的primary或者impersonation令牌。

当使用 CreateJobObject 创建job后，返回的handle具有JOB_OBJECT_ALL_ACCESS权限。

当使用 OpenJobObject，系统会检查请求访问权限。

如果一个job object在一个嵌套job层级里，调用者将具有子job的权限。

如果你想读写job object的SACL, 则需要请求一个job object的ACCESS_SYSTEM_SECURITY权限。

必须对job关联的每一个进程都设置安全限制，而不是设置job object本身。

## 示例代码

[ResLimitsOnWin on Github](https://github.com/ityuhui/ResLimitsOnWin)

## 参考

[Job Objects](https://docs.microsoft.com/en-us/windows/win32/procthread/job-objects)
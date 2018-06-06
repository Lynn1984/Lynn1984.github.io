---
title: WCF入门教程，即学即会
date: 2018-06-06 13:51:21
tags:
---

标签（空格分隔）： C#

---

## 什么是WCF

* WindowsCommunication Foundation(WCF)是由微软开发的一系列支持数据通信的应用程序框架，可以翻译为Windows 通讯开发平台。
* 整合了原有的windows通讯的 .net Remoting，WebService，Socket的机制，并融合有Http和Ftp的相关技术。
* 是Windows平台上开发分布式应用最佳的实践方式。

## WCF体系架构简介

### 契约与说明
    
契约定义消息系统的各个方面：

* 数据契约：服务中的参数；
* 消息契约：使用SOAP协议特定的消息部分；
* 服务契约：服务中的方法；
* 策略与绑定：策略设置安全或其他条件，绑定指定传输方式与编码。

### 服务运行时

服务运行期间的行为控制

* 限制行为：控制处理的消息数；
* 错误行为：出现内部错误时所处理的操作；
* 元数据行为：是否向外提供元数据及元数据的提供方式；
* 实例行为：可运行的服务实例数目；
* 事务行为：处理事务；
* 调度行为：控制WCF处理消息的方式；

### 消息传递

* 消息传递层：说明数据的交换格式和传输模式。
* 消息传递层由通道（信道）组成，通道是对消息进行处理的组件，负责以一致的方式对消息进行整理和传送。通道用于传输层、协议层、及消息获取。各层次的通道组成了信道栈。
* 通道对消息和消息头进行操作，服务运行时对消息正文进行操作。
* 两种类型：传输通道 与 协议通道。
* 传输通道：读取和写入来自网络的消息，传输通道通过编码器将消息转换为网络传输使用的字节流，以及将字节流转换为消息。传输通道示例如：HTTP通道、命名管道、TCP、MSMQ等；
* 协议通道：通过读取或写入消息头的方式来实现消息协议，协议通道示例如：WS-Security，WS-Reliability。

### 承载和激活

* 服务宿主： 负责WCF服务的生命周期和上下文的操作系统进程，负责启动和停止WCF服务，并提供控制服务的基本管理功能。


## WCF基础概念介绍

### 契约（Contract）

WCF 的基本概念是以合约(Contract)来定义双方沟通的协议，合约必须要以接口的方式来体现，而实际的服务代码必须要由这些合约接口派生并实现。合约分成了四种：

* 数据合约 (Data Contract)，订定双方沟通时的数据格式。
* 服务合约 (Service Contract)，订定服务的定义。
* 操作合约 (Operation Contract)，订定服务提供的方法。
* 消息合约 (MessageContract)，订定在通信期间改写消息内容的规范。一个WCF中的合约，就如同下列代码所示：

```
using System;  
using System.ServiceModel;  
namespace Microsoft.ServiceModel.Samples{  
  
[ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples")]// 服务合约  
public interface ICalculator  
{  
[OperationContract] // 操作合约  
double Add(double n1, double n2);  
  
[OperationContract] // 操作合约  
double Subtract(double n1, double n2);  
  
[OperationContract] // 操作合约  
double Multiply(double n1, double n2);  
  
[OperationContract] // 操作合约  
double Divide(double n1, double n2);  
}  
}  
```

### 绑定 (Binding)

由于 WCF 支持了 HTTP，TCP，Named Pipe，MSMQ，Peer-To-Peer TCP等协议，而 HTTP 又分为基本 HTTP 支持 (BasicHttpBinding)以及WS-HTTP支持(WsHttpBinding)，而 TCP亦支持NetTcpBinding， NetPeerTcpBinding等通信方式，因此，双方必须要统一通信的协议，并且也要在编码以及格式上要有所一致。

```
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
<system.serviceModel>  
<!-- 设定服务系结的资讯 -->  
<services>  
<service name=" CalculatorService" >  
<endpoint address="" binding="wsHttpBinding"bindingConfiguration="Binding1" contract="ICalculator"/>  
</service>  
</services>  
<!-- 设定通讯协定系结的资讯 -->  
<bindings>  
<wsHttpBinding>  
<binding name="Binding1">  
</binding>  
</wsHttpBinding>  
</bindings>  
</system.serviceModel>  
</configuration>  
```

### 终结点

–终结点是用来发送或接收消息（或执行这两种操作）的构造。终结点包括一个定义消息可以发送到的目的地的位置（地址）结点，包括一个定义消息可以发送到的目的地的位置（地址）、一个描述消息应如何发送的通信机制规范（绑定）以及对于可以在该位置发送或接收（或两者皆可）的一组消息的定义（服务协定）—该定义还描述了可以发送何种消息。

###  元数据

所谓的“元数据”就是描述数据的数据，即描述当前服务有哪些服务契约、方法契约和数据契约以及终结点的信息。而“元数据终结点”就是向外界暴露元数据的终结点。当客户端添加WCF服务引用的时候，会首先通过元数据取得服务器端的契约信息、终结点信息，然后根据这些信息在客户端创建了代理类，我们在客户端调用WCF服务的过程实际上就是通过代理类调用WCF服务的过程。

## 简单的实例

### 创建项目

* 新建立空白解决方案，并在解决方案中新建项目，项目类型为：WCF服务应用程序。

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/wcf/1.png)

* 建立完成后如下图所示：

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/wcf/2.png)

* 添加自定义的WCF服务文件。在项目中添加新项：

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/wcf/3.png)

* 添加代码

```
namespace WcfServiceDemo
{
    // 注意: 使用“重构”菜单上的“重命名”命令，可以同时更改代码和配置文件中的接口名“IUser”。
    [ServiceContract]
    public interface IUser
    {
        [OperationContract]
        string ShowName(string name);
    }
}
```

```
namespace WcfServiceDemo
{
    // 注意: 使用“重构”菜单上的“重命名”命令，可以同时更改代码、svc 和配置文件中的类名“User”。
    // 注意: 为了启动 WCF 测试客户端以测试此服务，请在解决方案资源管理器中选择 User.svc 或 User.svc.cs，然后开始调试。
    public class User : IUser
    {
        public string ShowName(string name)
        {
            string wcfName = string.Format("WCF服务，显示姓名：{0}", name);
            return wcfName;
        }
    }
}
```

[ServiceContract]:来说明接口是一个WCF的接口，如果不加的话，将不能被外部调用。
[OperationContract]:来说明该方法是一个WCF接口的方法，如果不加的话，将不能被外部调用。

* F5运行，双击ShowName方法

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/wcf/4.png)

### 发布

选择项目，在“生成”下面选“发布”：
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/wcf/6.png)
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/wcf/7.png)
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/wcf/8.png)
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/wcf/9.png)


### IIS 部署运行

右击“我的电脑”，选择“设备”，添加网站如下图：

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/wcf/5.png)

访问http://localhost:777

![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/wcf/10.png)
![Aaron Swartz](https://raw.githubusercontent.com/Lynn1984/Lynn1984.github.io/master/image/wcf/11.png)
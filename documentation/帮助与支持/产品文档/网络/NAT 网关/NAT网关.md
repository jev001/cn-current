## **NAT网关**

功能：同一VPC内的多台云主机同时有访问Internet的需求且公网IP资源不足时，可通过创建NAT网关解决此问题。京东云支持自建NAT网关，实现SNAT功能。

使用限制：自建NAT网关与云主机共用配额。

**概述**

NAT 网关是一种将私有网络中内网 IP 地址和公网 IP 地址进行转换的网关，是私有网络内无公网 IP 的云资源访问 Internet 的一种方式（但不支持 Internet 主动访问私有网络）。京东云私有网络NAT网关的典型应用场景如下：

* 
大带宽、高可用公网访问。针对用户需要超大带宽、公网 IP 使用量大、部署服务较多的公网访问应用场景，京东云 NAT 网关均可以满足需求。
* 
安全的公网访问。京东云私有网络的 NAT 网关提供 IP 的安全转换。如果用户希望隐藏私有网络内主机的公网 IP 以避免暴露其网络部署，同时又希望访问公网，那么使用京东云 NAT 网关可以满足这类需求。

**NAT网关和弹性公网IP的使用**

### **方案1：只使用NAT网关**

云主机不绑定弹性公网IP，所有访问Internet流量通过NAT网关转发。此种方案中，云主机访问Internet的流量会通过内网转发至NAT网关，因而不会受云主机购买时公网带宽的带宽上限限制，NAT网关产生的网络流量费用也不会占用云主机的公网带宽出口。

### **方案2：只使用弹性公网IP**

云主机只绑定弹性公网IP，不使用NAT网关。此种方案，云主机所有访问Internet流量通过弹性公网IP出，会受到云主机购买时公网带宽的带宽上限限制。访问公网产生的相关费用，根据云主机网络计费模式而定。

### **方案3：同时使用NAT网关和弹性公网IP**

云主机绑定了弹性公网IP，同时所在子网路由访问Internet流量指向了NAT网关。此种方案中，所有云主机主动访问Internet的流量只通过内网转发至NAT网关，回包也经过NAT网关返回至云主机，此部分流量不会受云主机购买时公网带宽的带宽上限限制，NAT网关产生的网络流量费用不会占用云主机的公网带宽出口。如果来自Internet的流量主动访问云主机的弹性公网IP，则云主机回包统一通过弹性公网IP返回，这样产生的公网出流量受云主机购买时公网带宽的带宽上限限制。访问公网产生的相关费用，根据云主机网络计费模式而定。

**NAT网关配置说明**

**目的**：实现相同VPC下的多台主机通过一台作为NAT网关的主机访问公网，共享IP带宽。

**模拟场景**：假定目前存在VPC名为NAT01（10.1.0.0/16），下属两个子网：SNAT01(10.1.1.0/24)、SNAT02(10.1.2.0/24)，SNAT02下存在多台主机，实现子网SNAT02下的多台主机可通过子网SNAT01下一台作为NAT GW的主机上网。

**步骤一：在SNAT01子网下创建一台作为NAT网关的主机**

![1.png](https://img1.jcloudcs.com/cms/6c6860b2-2e41-4e9b-a972-134b9ab33b2a20180413171603.png)

在SNAT01下创建一台主机，镜像选择CentOS-7.2 64位 NAT Gateway

![2.png](https://img1.jcloudcs.com/cms/67974b04-458d-46f8-829d-d0a4dab8e52120180413171631.png)

选择所属VPC为NAT01、子网为SNAT01

配置公网IP类型、带宽

主机命名为SNAT01

![3.png](https://img1.jcloudcs.com/cms/622f2ac2-60d0-4a1d-8ac8-799e365acbc920180413171656.png)

创建完成后，在主机列表页可以看到相同VPC下不同子网的主机情况

**步骤二：配置子网SNAT02****的路由表指向NAT****网关**

![4.png](https://img1.jcloudcs.com/cms/5b66d06f-c4da-4ce8-af25-0a0c66d4a8bd20180413171752.png)

通过子网SNAT02查看和子网绑定的路由表

![5.png](https://img1.jcloudcs.com/cms/5be5e2d7-374d-49b1-b23e-c197bdbc139920180413171818.png)

编辑路由表：下一跳类型：云主机，下一跳主机：SANT01

配置完成后，子网SNAT02下的所有主机都可以通过SANT01作为NAT网关进行公网访问。

同理，可配置相同VPC下其他的子网路由。

**注意：如主机需要通过NAT****网关通信，不能选择与主机所在子网绑定同一张路由表的子网内的主机作为NAT****网关。**
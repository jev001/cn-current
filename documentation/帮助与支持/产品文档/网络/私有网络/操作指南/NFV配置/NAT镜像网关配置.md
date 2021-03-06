# **NAT镜像网关配置**

****

## **一、创建NAT Gateway的云主机**

1. 
进入京东云控制台云主机页面，选择创建云主机，进入云主机的创建流程。
1. 
选择CentOS 7.2 64位 NAT Gateway镜像及主机其他相应的参数，进行云主机创建。
1. 
创建成功，相关信息更新；创建失败，则出现提示框，如果多次申请失败，请联系客服。

### ![图1.png](https://img1.jcloudcs.com/cms/76e24ec5-ad41-429d-805f-755063a9408b20170921153651.png)

## **二、配置路由表路由规则**

1. 
进入京东云控制台相应VPC路由表的详情页面，选择新建路由策略。
1. 
添加路由策略，目的端为公网地址，下一跳类型为云主机，下一跳选择之前通过NAT Gateway镜像创建的云主机。
1. 
将该路由表关联到有访问Internet的需求的实例所在的子网上。需要注意NAT Instance不能在此子网内，即子网内云主机只能通过同一私有网络下不同子网内的NAT Instance访问公网。

![图2.png](https://img1.jcloudcs.com/cms/c927d3d0-4d04-48e4-9f11-50d4a84d846e20170921153707.png)

![图3.png](https://img1.jcloudcs.com/cms/ff77199c-f746-4054-a8e9-ea9bc0df0b0620170921153714.png)
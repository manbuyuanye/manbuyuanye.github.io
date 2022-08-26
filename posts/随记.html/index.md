# 随记

## SVI
Cisco多层交换中提到了一个SVI接口，路由接口。在多层交换机上可以将端口配置成不同类型的接口。

其中SVI接口 类似于  
interface Vlan10  
ip address 192.168.20.1 255.255.255.0 secondary  
ip address 192.168.10.1 255.255.255.0


路由接口类似于  
interface FastEthernet 0/1  
ip address 192.168.0.1 255.255.255.0  
ip address 192.168.1.1 255.255.255.0 seconfary  

**svi接口说白了就是vlan接口，在上面配上ip，然后把二层端口加入进来，以前的设备不能直接在端口 上配ip，就用svi接口方式来配ip，实现三层连接。我们现在在实际工程里面，如果支持端口直接配ip，就不用svi方式。那种老6500系列是2 层，3层分开的，那种就得用svi接口来进行3层连接。**  
--https://www.cnblogs.com/kscnchina/p/3277401.html

## 接口模式
二层接口有access、trunk和hybrid模式；三层接口没有这三种模式。  

## 单臂路由
当只有二层交换机时，又要实现不同vlan间通信时，需要用到单臂路由技术。

## port link-mode bridge/route
出现在华三网络设备中。  
port link-mode命令用来切换端口的工作模式。  
bridge：工作在二层模式（一般交换机默认是二层）  
route：工作在三层路由口模式（一般路由器默认是三层路由模式）

## combo enable copper/fiber
出现在华三网络设备中。  
Combo 接口是一个逻辑接口，一个 Combo 接口对应设备面板上一个电口和一个光口。电口与其对应的光口是光电复用关系，两者不能同时工作（当激活其中的一个接口时，另一个接口就自动处于禁用状态），用户可根据组网需求选择使用电口或光口。  
combo enable copper 电口被激活  
combo enable fiber 光口被激活  

## 路由器接口
一般来讲，路由器一个接口连接一个网络，路由器有多个接口，所以可以连接多个网络，用以网络间通信。



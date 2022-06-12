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

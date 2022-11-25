# 网络小记


# 1、路由协议
## 1.1 路由表
路由表是基于IPv4或IPv6的指导网络流量走向的数据表。  
基本参数：目的网络、来源、下一跳、出接口、优先级、度量值  
路由优先级：  
| 路由协议| 外部优先级 |
| ---- | ---- |
| FDirect |  0 |
| OSPF | 10 |
| IS-IS | 15 |
| Static | 60 |
| RIP | 100 |
| OSPF ASE | 150 |
| OSPF NSSA | 150 |
| IBGP | 255 |
| EBGP | 255 |
## 1.2 直连路由

## 1.3 静态路由
静态路由是手工填写的路由表，一般指定了目的网络、下一跳，出接口、优先级可以按需配置。  
## 1.4 OSPF

## 1.5 BGP
### IBGP

### EBGP
## 1.6 IGRP/EIGRP

## 1.7 策略路由

## 1.8 RIP

## 1.9 IS-IS

# 2、VPN
## 2.1 GRE

## 2.2 IPsec

## 2.3 SSL VPN

# 3、隧道技术
## 3.1 MPLS VPN

## 3.2 VxLAN

# 4、STP
## 4.1 STP
1、STP的角色和状态  
STP的设备角色：根桥、非根桥。  
STP的端口角色：根端口、指定端口、预备端口。  
STP的端口状态：init、blocking、listening、learning、forwarding、disable。  

2、STP实验1  
实验拓扑：  
![STP.png](https://s2.loli.net/2022/11/26/T6uND1scWkoj7KH.png)
实验配置：  
```
AR1:
interface GigabitEthernet0/0/0
 ip address 192.168.10.1 255.255.255.0 
interface GigabitEthernet0/0/1
 ip address 192.168.20.1 255.255.255.0 
interface LoopBack0
 ip address 1.1.1.1 255.255.255.255 

LSW1：
stp mode stp
stp instance 0 root primary
interface GigabitEthernet0/0/1
 port hybrid pvid vlan 10
 port hybrid untagged vlan 10 20
interface GigabitEthernet0/0/2
 port link-type trunk
 port trunk allow-pass vlan 10 20
interface GigabitEthernet0/0/3
 port hybrid pvid vlan 10
 port hybrid untagged vlan 10 20

LSW2：
stp mode stp
stp instance 0 root secondary
interface GigabitEthernet0/0/1
 port hybrid pvid vlan 20
 port hybrid untagged vlan 10 20
interface GigabitEthernet0/0/2
 port link-type trunk
 port trunk allow-pass vlan 10 20
interface GigabitEthernet0/0/3
 port hybrid pvid vlan 20
 port hybrid untagged vlan 10 20

LSW3：      
stp mode stp
interface GigabitEthernet0/0/1
 port hybrid pvid vlan 10
 port hybrid untagged vlan 10 20
interface GigabitEthernet0/0/2
 port hybrid pvid vlan 20
 port hybrid untagged vlan 10 20
interface GigabitEthernet0/0/3
 port hybrid pvid vlan 10
 port hybrid untagged vlan 10 20
 stp edged-port enable
interface GigabitEthernet0/0/4
 port hybrid pvid vlan 20
 port hybrid untagged vlan 10 20
 stp edged-port enable
说明：   
stp edge-port作用：使端口变为边缘端口，不参与stp计算，加快stp收敛速度。  
```
STP状态：  
```
<LSW1>dis stp bri
 MSTID  Port                        Role  STP State     Protection
   0    GigabitEthernet0/0/1        DESI  FORWARDING      NONE
   0    GigabitEthernet0/0/2        DESI  FORWARDING      NONE
   0    GigabitEthernet0/0/3        DESI  FORWARDING      NONE
<LSW2>DIS STP BRI
 MSTID  Port                        Role  STP State     Protection
   0    GigabitEthernet0/0/1        DESI  FORWARDING      NONE
   0    GigabitEthernet0/0/2        ROOT  FORWARDING      NONE
   0    GigabitEthernet0/0/3        DESI  FORWARDING      NONE
<LSW3>DIS STP BRI
 MSTID  Port                        Role  STP State     Protection
   0    GigabitEthernet0/0/1        ROOT  FORWARDING      NONE
   0    GigabitEthernet0/0/2        ALTE  DISCARDING      NONE
   0    GigabitEthernet0/0/3        DESI  FORWARDING      NONE
   0    GigabitEthernet0/0/4        DESI  FORWARDING      NONE

```
实验结果：PC1和PC2到AR1通。  

3、STP实验2  
实验拓扑：  
![STP实验2.png](https://s2.loli.net/2022/11/26/NUFSbZavWEhQfVC.png)  
实验配置：  
```
AR1-1：
interface GigabitEthernet0/0/0
 ip address 192.168.10.1 255.255.255.0 
interface LoopBack0
 ip address 1.1.1.1 255.255.255.255 

LSW1-1：
stp disable
interface Eth-Trunk1
 port link-type trunk
 port trunk allow-pass vlan 10 20
interface GigabitEthernet0/0/1
 port link-type access
 port default vlan 10
interface GigabitEthernet0/0/2
 eth-trunk 1
interface GigabitEthernet0/0/3
 eth-trunk 1

LSW3-1：  
stp disable
interface Eth-Trunk1
 port link-type trunk
 port trunk allow-pass vlan 10 20
interface GigabitEthernet0/0/1
 eth-trunk 1
interface GigabitEthernet0/0/2
 eth-trunk 1
interface GigabitEthernet0/0/3
 port link-type access
 port default vlan 10
interface GigabitEthernet0/0/4
 port link-type access
 port default vlan 20

```
实验结果：PC1-1到AR1-1通，PC2-1到AR1-1不通。说明：链路聚合时，聚合的链路可以看作是一条链路，聚合的链路无需考虑成环问题。  

## 4.2 RSTP

## 4.3 MSTP

# 5、高可靠技术
## 5.1 设备级
### 堆叠

### 5.2 MLag

### 5.3 VRRP

## 5.2 链路级
### 手工链路聚合

### 静态链路聚合

### 动态链路聚合


1、DHCP续租

有50%和87.5%两个时间点。

客户端获得DHCP租约后，开始工作。租期达到50%时，对目前的DHCP服务器发送DHCP Request以请求更新DHCP租约，若未得到回复，租期达到87.5%时，对网络内广播DHCP Discover，以请求其它DHCP服务器的租约。租期届满未收到任何回复，DHCP租约取消。大致过程如下：

| 0| 0%<t<50% | 50%<=t<87.5% | >=87.5% | 
| ---- | ---- | ---- | ---- | 
| init | 正常工作 | DHCP Request | DHCP Discover | 

2、IPv6 Solicited Node Multical Address 被请求节点组播地址



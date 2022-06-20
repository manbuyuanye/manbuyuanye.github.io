# 华三命令参考

## 1.SCF（安全集群架构）
SCF典型配置方法

1. 配置主设备

a) 配置主框的成员编号、优先级，并设置主框为SCF模式，设置完毕后系统会进行重启：

[H3C]irf member 1

[H3C]irf priority 32

[H3C]chassis convert mode irf

b) 重启后，系统变为四维模式，进行如下配置使能SCF系统启动文件的自动加载功能：

[H3C]irf auto-update enable

c) 然后，配置设备的SCF端口，对应的物理端口应处于shutdown状态：

[H3C]irf-port 1/2

[H3C-irf-port1/2]port group interface Ten-GigabitEthernet1/2/0/7

[H3C-irf-port1/2]port group interface Ten-GigabitEthernet1/2/0/8

2. 配置从设备

a) 配置主框的成员编号、优先级，并设置主框为SCF模式，设置完毕后系统会进行重启：

[H3C]irf member 2

[H3C]irf priority 1

[H3C]chassis convert mode irf

b) 重启后，系统变为四维模式，进行如下配置使能SCF系统启动文件的自动加载功能：

[H3C]irf auto-update enable

c) 然后，配置设备的SCF端口，对应的物理端口应处于shutdown状态;配置完成后，开启对应的物理端口：

[H3C]irf-port 2/1

[H3C-irf-port2/1]port group interface Ten-GigabitEthernet2/2/0/7

[H3C-irf-port2/1]port group interface Ten-GigabitEthernet2/2/0/8

3. 激活SCF配置

在主、从设备分别激活SCF配置，协商后从设备自动重启动，SCF部署成功：

[H3C]irf-port-configuration active

### 举例：
```
主设备：

int 10g1/0/2
shutdown
int 10g1/0/3
shutdown

irf member 1 priority 32
irf auto-update enable

irf-port 1/2
port group int 10g1/0/2
port group int 10g1/0/3
irf-port-configuration active

int 10g1/0/2
undo shutdown
int 10g1/0/3
undo shutdown

从设备：

int 10g1/0/2
shutdown
int 10g2/0/3
shutdown

irf member 1 renumber 2
irf member 2 priority 1
irf auto-update enable

irf-port 2/1
port group int 10g2/0/2
port group int 10g2/0/3
irf-port-configuration active

int 10g2/0/2
undo shutdown
int 10g2/0/3
undo shutdown

```

## 链路聚合
### 二层静态聚合
```
[SWB]int bagg 1
[SWB-Bridge-Aggregation1]qu
[SWB]int range g1/0/1 to g1/0/3
[SWB-if-range]port link-aggregation group 1
[SWB-if-range]qu
[SWB]int bagg 1
[SWB-Bridge-Aggregation1]port link-type trunk
[SWB-Bridge-Aggregation1]port trunk permit vlan 10 20

```
注意：必须先创建bagg端口，再加入现有端口，最后设置bagg端口属性。加入现有端口和设置bagg端口属性的顺序不能颠倒。
### 二层动态聚合
```
[SWB]int bagg 1
[SWB]link-aggregation mode dynamic
[SWB-Bridge-Aggregation1]qu
[SWB]int range g1/0/1 to g1/0/3
[SWB-if-range]port link-aggregation group 1
[SWB-if-range]qu
[SWB]int bagg 1
[SWB-Bridge-Aggregation1]port link-type trunk
[SWB-Bridge-Aggregation1]port trunk permit vlan 10 20
```
### 二层聚合负载分担

### 二层聚合边缘端口

### 三层静态聚合

### 三层动态聚合


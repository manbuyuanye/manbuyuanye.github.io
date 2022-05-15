# 组播和IGMP

## 组播
### 组播定义和地址结构（IP and MAC）
#### 定义
组播是在一台源IP主机和多台IP主机之间进行，中间的网络设备根据接收者的需要，有选择性地对数据进行复制和转发。  
#### 组播IP地址
| 组播目的MAC | 组播源MAC | 组播目的IP | 组播源IP | payload |
| ---- | ---- | ---- | ---- | ---- |
| dest mac | src mac | dest ip | src ip | payload |

组播报文和单播报文类似.
组播源IP和MAC是源服务器的**单播**IP和MAC。
组播目的IP范围：224.0.0.0~239.255.255.255
组播目的MAC由组播IP映射而来。
D类地址用于组播，D类地址的进一步分类：  
|序号|范围|定义|
|---|---|---|
|①|224.0.0.0~224.0.0.255|为协议预留的永久组地址|
|②|232段|Source-Specific临时组播组地址|
|③|除①②外的224-238段|Any-Source临时组播组地址|
|④|239段|本地管理的Any-Source临时组播组地址|  
真正可用的其实是③。
#### 组播MAC地址
组播MAC地址：最高字节的最低比特位为1。一个组播MAC可能对应多个组播IP。  
IPv4组播MAC地址：前24位为0x01005e，第25位为0，后23位等于组播IP的后23位。  
IPv6组播MAC地址：前16位为0x3333，后32位等于组播IPv6的后32位。  
  
MAC地址前两位  
![MAC.jpg](https://s2.loli.net/2022/05/15/kE3AnNdI1YGWzB4.jpg)  
MAC地址最高字节的最低位  
![组播MAC地址.png](https://s2.loli.net/2022/05/15/uhQGifc4Iw1ST36.png)  
组播IPv4与组播MAC映射关系  
![组播IPv4与组播MAC映射关系.png](https://s2.loli.net/2022/05/15/aLk7rMObq5H8FDN.png)  
组播IPv6与组播MAC映射关系  
![组播IPv6与组播MAC映射关系.png](https://s2.loli.net/2022/05/15/2e8KZ1aONmhpUA3.png)
#### 其他
组播服务模型，分为：ASM（Any-Source Model，任意源服务模型）和SSM（Source-Specific，指定源服务模型）。ASM，源IP任意，目的组播IP唯一。SSM，源IP唯一，目的组播IP任意。
### 数据的转发原理
#### RPF
RPF,Reverse Path Forwarding,反向路由转发
组播数据存在的问题：①环路；②转发时采用了次优路径。
### 反向路径转发的原理

### 其他
冲突域是第一层
广播域是第二层


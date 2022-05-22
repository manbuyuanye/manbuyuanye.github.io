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
组播转发核心：组播路由表项、RPF、组播路由协议。  
1、组播数据存在的问题：①环路；②转发时采用了次优路径。  
组播数据转发的要求：无环、无次优路径、无重复包。  
达到以上要求的手段：RPF机制和组播路由协议。  
达到以上要求的路径称为**组播分发树**--组播源为根、组成员为叶子。  
2、RPF,Reverse Path Forwarding,反向路径转发。  
组播路由协议：PIM、MSDP、MBGP、IGMP。  
工作在**组播转发网络**：PIM、MSDP、MSGP。  
工作在**成员端网络**：IGMP。  
**PIM**，Protocol Independent Multicast,协议无关组播，主要作用是**生成AS域内的组播分发树**。  
**MSDP**，Multicast Source Discovery Protocol，组播源发现协议，主要作用是**帮助生成AS域间的组播分发树**。  
**MBGP**，Multicast BGP，组播BGP，主要作用是**帮助跨域组播流进行RPF校验**。  
**IGMP**，Internet Group Management Protocol互联网组管理协议，主要作用是**告知组播网络，组成员的位置与所加组播组**。  
3、**组播路由表项--（S,G）表**
|组播信息|入接口|出接口|
|-|-|-|
|（S,G）|IF1|IF2|  

（S,G）就是（组播源，组播组），如：（1.1.1.1,224.1.1.1）
S在**单播路由表或MBGP路由表或组播静态路由表中对应的出接口**必须等于**组播路由表项中的入接口(IF1)**,若相等，那么RPF检查通过，否则RPF不通过。若同时存在单播路由、MBGP路由和组播静态路由，此时组播静态路由>MBGP路由>单播路由。

### 其他
冲突域是第一层  
广播域是第二层
## IGMP
### IGMP基本原理和配置
1、IGMP两张表：IGMP组表项、IGMP路由表项。  
**IGMP组表项**
|组播组|
|---|
|G1|
|G2|

**IGMP路由表项**
|组播组|出接口|
|---|---|
|G1|IF1|
|G2|IF2|  

2、在最后一跳组播路由器（组播叶子路由器）上，IGMP组表项、IGMP路由表项、PIM路由表（组播协议路由表）可以汇总后形成组播路由表。  
其中，**IGMP组表项**又可为**PIM路由表**提供**组播组地址**信息，**IGMP路由表项**又可为**PIM路由表**提供**出接口**信息。  


### IGMPv1、v2、v3
#### IGMPv1
**1、原理：**v1基于**查询和响应机制**完成组播组管理。  
**2、查询和响应机制**  
普遍组查询报文（General Query）：IGMP查询器发送General Query给所有主机和路由器，用户查询**组播组及其成员**。该报文为组播报文。  
成员关系报告报文（Report）：主机收到General Query后，发送Report，用于**申请加入某个组播组**或者**应答查询报文**。该报文为组播报文。  
**3、IGMPv1普遍组查询报文与成员关系报告报文格式：**  
|||||
| :-: | :-: | :-: | :-: |
|Version|Type|Unused|Checksum|
|Group Address|


最主要的三个字段：  
Version：IGMP版本，值为1。  
Type：报文类型。0x11：普遍组查询报文；0x12：成员关系报告报文。  
Group Address：组播组地址。Type为0x11时，该字段为0；Type为0x12时，该字段为成员加入的组播组地址。  
**4、IGMP查询器**  
一个多路访问网络里，只需一个组播路由器发送General Query，该路由器称为**IGMP查询器**。
IGMP查询器选举机制：IGMPv1将PIM选举的唯一的**组播信息转发者（Assert Winner或DR）**作为**IGMPv1查询器**。  
IGMP查询器和非查询器均能收到成员关系报告报文，因此均能形成IGMP组表项和IGMP路由表项。（一个查多个收）  
IGMPv1组成员需要离组时，不会发离开组消息，也不会回应普遍组查询报文。  
#### IGMPv2
**1、IGMPv2报文格式**  
||||
| :-: | :-: | :-: |
|Type|Max Response Time|Checksum|
|Group Address|

Type：0x11是查询报文；0x12是IGMPv1成员关系报文；0x16是IGMPv2成员关系报文；0x17是成员离开报文。  
Max Response Time：普遍组查询默认为10秒，特定组查询默认为1秒。  
Group Address：普遍组查询报文中为**0**；特定组查询报文中为**该特定组地址**；成员关系报告或者离开组的报文中为**需要报告或者离开的地址**。  
**2、IGMPv2与v1的相同点**  
v2与v1组成员加组机制基本相同；  
v2与v1兼容。  
**3、IGMPv2相比v1的改进**  
v2增加了离开组机制；  
v2增加了查询器选举机制。  
**4、IGMPv2组成员离开机制**
为了IGMP改善组成员离开机制，v2相比v1新增两种报文格式：
**特定组查询报文（Group-Specific Query）**：用于查询器查询特定组播组是否存在成员，目的地址为该组播组的组播地址。  
**成员离开报文（Leave）**：组成员离开组播组时对查询器发出的报文，用于宣告自己离开了特定组播组，目的地址为224.0.0.2。  
**5、IGMPv2查询器选举机制**  
接口IP地址最小的路由器成为查询器。  
选举出查询器后，非查询器会启动一个定时器（Other Querier Present Timer），只要在超时时间内收到查询器的查询报文，就重置该定时器；否则，认为该查询器失效，重新发起查询器选举过程。  
#### IGMPv3
**1、IGMPv3与v2的相同点**  
查询器选举机制一致；  
使用普遍组查询报文查询组成员加组信息；  
使用特定组查询报文查询特定组播的成员信息。  
**2、IGMPv3的改进**  
新增**特定源组查询报文（Group-and-Source-Specific Query）**；  
成员关系报告报文新增**想要接收的来自哪些组播源的数据**；  
删除成员关系报告报文抑制机制；  
没有定义专门的成员离开报文，成员离开通过特定类型的报告报文来传达。  
**3、查询报文格式**  
||||||
| :-: | :-: | :-: | :-: | :-: |
|Type|Max Response Code|Checksum|
|Group Address|
|Resv|S|QRV|QQIC|Number of Source(N)|
|Source Address(1)|
|Source Address(2)|
|...|
|Source Address(N)|

字段说明：  
Type：0x11；  
Max Response Code：最大响应时间，成员主机在收到查询报文后的回应时间要求；  
Group Address：组播组地址，在**普遍组查询报文**中该字段为**0**，在**特定组查询报文**和**特定源组查询报文**中该字段为**要查询的特定组播组地址**。  
Number of Source：报文中包含的组播源数量，在**普遍组查询报文**和**特定组查询报文**中该字段为**0**，在**特定源组查询报文**中该字段**非0**，该字段值大小受到所在网络MTU限制。  
**4、成员关系报告报文格式**  

**5、组成员加组机制**  

**6、组成员离开机制**  

#### IGMP版本差异
![IGMP版本.jpg](https://s2.loli.net/2022/05/23/RqPGe7EcgtDS982.jpg)  
来自HCIP，若是有侵权可删
### IGMP Snooping

### IGMP SSM Mapping

### IGMP代理

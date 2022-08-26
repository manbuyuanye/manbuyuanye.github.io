# 网络小记


1、DHCP续租

有50%和87.5%两个时间点。

客户端获得DHCP租约后，开始工作。租期达到50%时，对目前的DHCP服务器发送DHCP Request以请求更新DHCP租约，若未得到回复，租期达到87.5%时，对网络内广播DHCP Discover，以请求其它DHCP服务器的租约。租期届满未收到任何回复，DHCP租约取消。大致过程如下：

| 0| 0%<t<50% | 50%t=<87.5% | >=87.5% | 
| ---- | ---- | ---- | ---- | 
| init | 正常工作 | DHCP Request | DHCP Discover | 

2、IPv6 Solicited Node Multical Address 被请求节点组播地址



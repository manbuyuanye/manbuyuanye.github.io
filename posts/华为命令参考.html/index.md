# 华为命令参考

## 用户
### console登录配置
password登录
```
[AR1]user-interface console 0
[AR1-ui-console0]authentication-mode password 
Please configure the login password (maximum length 16):password123
[AR1-ui-console0]
[AR1-ui-console0]
[AR1-ui-console0]
[AR1-ui-console0]dis th
[V200R003C00]
#
user-interface con 0
 authentication-mode password
 set authentication password cipher %$%$'8u@M;EVl1Tdi!KG6pO4,.@*0Im|@I-Rf,4"/.~]
gs;0.@-,%$%$
user-interface vty 0 4
user-interface vty 16 20
#
return

[AR1-ui-console0]return
<AR1>save
```

aaa登录
```
[AR2]user-interface console 0
[AR2-ui-console0]authentication-mode aaa
[AR2-ui-console0]aaa
[AR2-aaa]local-user wangdaye password cipher password123
[AR2-aaa]local-user wangdaye privilege level 15
[AR2-aaa]local-user wangdaye service-type terminal
[AR2-aaa]
[AR2-aaa]
[AR2-aaa]user-interface console 0
[AR2-ui-console0]dis th
[V200R003C00]
#
user-interface con 0
 authentication-mode aaa
user-interface vty 0 4
user-interface vty 16 20
#
return
[AR2-aaa]dis th
[V200R003C00]
#
aaa 
 authentication-scheme default
 authorization-scheme default
 accounting-scheme default
 domain default 
 domain default_admin 
 local-user admin password cipher %$%$K8m.Nt84DZ}e#<0`8bmE3Uw}%$%$
 local-user admin service-type http
 local-user wangdaye password cipher %$%$m[Ks7+[DV!=#vB=z$H[<tPLi%$%$
 local-user wangdaye privilege level 15
 local-user wangdaye service-type terminal
#
return
[AR2-aaa]ret
<AR2>save
```
### ssh登录
配置vty接口
```
[AR2]user-interface vty 0 4
[AR2-ui-vty0-4]authentication-mode aaa
[AR2-ui-vty0-4]protocol inbound ssh
```
配置aaa用户
```
[AR2-ui-vty0-4]aaa
[AR2-aaa]local-user sshuser password cipher password123
[AR2-aaa]local-user sshuser privilege level 15
[AR2-aaa]local-user sshuser service-type ssh terminal
[AR2-aaa]local-user sshuser idle-timeout 10
[AR2]ssh user sshuser authentication-type password 
```
配置ssh
```
[AR2]stelnet server enable 
[AR2]rsa local-key-pair create 
<AR2>save
```
注：若在网络设备AR1登录AR2，则AR1需要作为ssh客户端，才能登陆AR2
```
[AR1]ssh client first-time enable
```

## 端口范围设置
```
port-group group-member g 0/0/1 to g0/0/10
设置vlan
port default vlan 10
port trunk pvid vlan 10
port hybrid pvid vlan 10
设置连接类型
port link-type access
port link-type trunk
port link-type hybrid
```
## IGMP配置
```

```

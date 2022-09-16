# 网络配置实例

## 1、登录

### vty登录
配置aaa用户
```
aaa
local-user sshuser password cipher password123
local-user sshuser privilege level 15
local-user sshuser service-type ssh terminal https
local-user sshuser idle-timeout 10
ssh user sshuser authentication-type password 
```
配置ssh
```
stelnet server enable 
rsa local-key-pair create 
```
配置vty接口
```
user-interface vty 0 4
authentication-mode aaa
protocol inbound ssh
```
注：若在网络设备AR1登录AR2，则AR1需要作为ssh客户端，才能登陆AR2
```
ssh client first-time enable
```

### console登录配置

用户名密码登录
```
aaa
local-user wangdaye password cipher password123
local-user wangdaye privilege level 15
local-user wangdaye service-type terminal

user-interface console 0
authentication-mode aaa
```

password登录
```
user-interface console 0
authentication-mode password 
提示：Please configure the login password (maximum length 16):password123
```

## 2、端口范围设置
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


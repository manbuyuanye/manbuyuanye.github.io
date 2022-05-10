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


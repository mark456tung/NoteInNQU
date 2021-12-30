* r1
```
Router>en
Router#conf t
Router(config)#hostname r1
r1(config)#int e0/0
r1(config-if)#ip addr 12.1.1.1 255.255.255.0
r1(config-if)#no shut
r1(config-if)#exit
r1(config)#router rip
r1(config-router)#network 12.0.0.0
r1(config-router)#do show ip int brief
r1(config)#do show ip route
r1(config)#exit
----等r2 r3設置完成 ----
r1#ping 23.1.1.3

```
* r2
```
Router>en
Router#conf t
Router(config)#hostname r2
r2(config)#int e0/0
r2(config-if)#ip addr 12.1.1.2 255.255.255.0
r2(config-if)#no shut
r2(config-if)#int
r2(config-if)#int e0/1
r2(config-if)#ip addr 23.1.1.2 255.255.255.0
r2(config-if)#no shut
r2(config-if)#exit
r2(config)#router rip
r2(config-router)#network 12.0.0.0
r2(config-router)#network 23.0.0.0
r2(config-router)#do show ip int brief
```
* r3
```
Router>en
Router#conf t
Router(config)#hostname r3
r3(config)#int e0/0
r3(config-if)#ip addr 23.1.1.3 255.255.255.0
r3(config-if)#no shut
r3(config-if)#exit
r3(config)#router rip
r3(config-router)#network 23.0.0.0
r3(config)#do show ip route
```
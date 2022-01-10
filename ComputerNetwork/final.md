switch set vlan
```
Switch(config)#vlan 10
Switch(config-vlan)#exit
Switch(config)#vlan 20
Switch(config-vlan)#exit
Switch(config)#int e0/0
Switch(config-if)#switchport trunk encapsulation dot1q
no sh
Switch(config-if)#switchport mode trunk
Switch(config-if)#int e0/1
Switch(config-if)#switchport access vlan 10
Switch(config-if)#switchport mode access
no sh
Switch(config-if)#int e0/2
Switch(config-if)#switchport access vlan 20
Switch(config-if)#switchport mode access
```
router 


int e0/0.10
encapsulation dot1Q 10
ip addr 192.168.10.254 255.255.255.0
no shut
ip dhcp pool mypool1
network 192.168.10.0 255.255.255.0
default-router 192.168.10.254 
dns-server 8.8.8.8
ip dhcp excluded-address 192.168.10.250 192.168.10.254

int e0/0.20
encapsulation dot1Q 20
ip addr 192.168.20.254 255.255.255.0
no shut
ip dhcp pool mypool2
network 192.168.20.0 255.255.255.0
default-router 192.168.20.254 
dns-server 8.8.8.8
ip dhcp excluded-address 192.168.20.250 192.168.20.254



1
//r1 
ip route 0.0.0.0 0.0.0.0 12.1.1.2
//r2
ip route 192.168.1.0 255.255.255.0 12.1.1.1
ip route 192.168.2.0 255.255.255.0 12.1.1.1
ip route 192.168.3.0 255.255.255.0 12.1.1.1
ip route 0.0.0.0 0.0.0.0 23.1.1.3
access-list 1 permit 192.168.1.0 0.0.0.255
access-lset 2 permit 192.168.2.0 0.0.0.255
access-lset 3 permit 192.168.3.0 0.0.0.255
int e0/0
ip nat inside
int e0/1
ip nat outside
exit
ip nat inside soucre list 1 pool PAT overload
ip nat inside soucre list 2 pool PAT overload
ip nat inside soucre list 3 pool PAT overload

2.
r2
access-list 1 deny icmp 192.168.1.0 0.0.0.255 host 1.2.3.5
access-list 1 permit any
int e0/0
ip access-group 1 in
3.
r3
access-list 100 deny icmp 192.168.2.0 0.0.0.255 host 1.2.3.4
access-list 1 permit any
int e0/1
ip access-group 1 out
4 
r2 
ip nat inside source static http 192.168.3.1 80 23.1.1.2 80
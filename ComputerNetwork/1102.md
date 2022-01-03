# 2021.11.2
eigrp 由cisco 提出  
路由協定可分為 distance-vector和 link-state  
loop-free 環路依然存在，但不會經過  
想要知道介面的資訊$show interfaces 介面名稱

* r1
```
en 
conf t
hostname R1
int e0/0
ip addr 12.1.1.1 255.255.255.0
no shut
exit
```
* r2
```
en
conf t
hostname R2
int e0/0
ip addr 12.1.1.2 255.255.255.0
no shut
int e0/1
ip addr 23.1.1.2 255.255.255.0
no shut
```
* r3
```
en 
conf t
hostname R3
int e0/0
ip addr 23.1.1.3 255.255.255.0
no shut
```
* r1
```
router eigrp 10
network 12.1.1.0 0.0.0.255
```
* r2
```
router eigrp 10
network 12.1.1.0 0.0.0.255
network 23.1.1.0 0.0.0.255
```
* r3
```
router eigrp 10
network 23.1.1.0 0.0.0.255
```
> show ip eigrp neighbors

> show ip eigrp topology

* r1
 ```
 en
 conf t
 int lo1
 ip adr 172.16.0.1 255.255.255.0
 router eigrp 10
 network 172.16.0.0 0.0.0.255
 ```
 * r3
 ```
 en
 conf t
 int lo1
 ip addr 172.16.1.1 255.255.255.0
 router eigrp 10
 network 172.16.1.0 0.0.0.255
 ```
 > no auto-summary

 ### 手動合併網路
 * r1
 ```
 en
 conf t
 int lo2
 ip addr 172.16.4.1 255.255.255.0
 ip addr 172.16.5.1 255.255.255.0 secondary
 int e0/0
 ip summary address eigrp 10 172.16.4.0 255.255.254.0
 router eigrp 10
 network 172.16.4.0 0.0.0.255
 network 172.16.5.0 0.0.0.255
 ```
 ### 認證的實驗
 * r1
 ```
 en
 conf t
 key chain Mychain
 key 1
 key string 123456
 int e0/0
 ip authentication key-chain eigrp 10 Mychain
 ip authentication mode eigrp 10 md5
 ```
 * r2
 ```
 en
 conf t
 key chain Mychain
 key 1
 key string 123456
 int e0/0
 ip authentication key-chain eigrp 10 Mychain
 ip authentication mode eigrp 10 md5
 ```
 
* > :user mode
* \# :privilege mode 輸入enable 進入特權模式
* config :golbal configuration mode 輸入config t
* 都是用exit回到上層 在接口想直接回到privilege mode 可以用end
* nvram 中有一個start configure 剛開機的時候會去讀取它 若要重開時配置還在，要把running configure 寫回去start configure
ping的時候 !代表封包傳送成功
第一個封包要arp解析(要網路卡卡號)通常會失敗
```
在router中設定
interface e0/0
ip addr 192.168.1.254 255.255.255.0
no shut
interface e0/1
ip addr 192.168.2.254 255.255.255.0
no shut
do show ip interface brief
在pc1 ip 192.168.1.1 255.255.255.0 192.168.1.254
  pc2 ip 192.168.2.1 255.255.255.0 192.168.2.254
  從pc2 ping 192.168.1.1 or pc1 ping 192.168.1.1
 ``` 
 ```
 設定dhcp
 在router
 ip dhcp pool mypool
 netwrok 192.168.1.0 255.255.255.0
 default-router 192.168.1.254 
 dns-server 8.8.8.8
 dns-server exclude 192.168.1.250 192.168.1.254 (排除一些不想分配出去的ip)
 
 ip dhcp pool mypoo2
 netwrok 192.168.2.0 255.255.255.0
 default-router 192.168.2.254
 dns-server 8.8.8.8
 dns-server exclude 192.168.2.250 192.168.2.254
 
 clear ip 清除ip
 pc端中 打 ip dhcp
 ```
 ```
 
 在router中
 int e0/2
 ip addr dhcp
 no shut
 do show ip int brief
 username cisco password cisco
 enable secret cisco
 ip domain-name test.com
 hostname R1
 crypto key generate rsa
 ip ssh version 2
 line vty 0 4   (設置遠端可以有五個帳戶登入0~4)                        
 login local                          
 transport input ssh  
 ```
 enable ?
 password 密碼明文顯示在configure
 secret 密碼在configure中會加密
 show ip dhcp binding顯示shcpserver 送出的資訊
-line vty
看設定備配置
show running-configure
設定配置(讓重開後配置不會跑掉)
write memory
ctrl+a 可以讓游標移動到最左邊，ctrl+可以讓游標移到最右邊(linux也是)

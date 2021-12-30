# 2021.10.05
## TFTP
* 簡單檔案傳輸協定也稱小型檔案傳輸協定（Trivial File Transfer Protocol, TFTP）。
* 透過少量記憶體就能輕鬆實現

## OpenWrt
* OpenWrt是適合於嵌入式裝置的一個Linux發行版。
* 不是一個單一、靜態的韌體，而是提供了一個可添加軟體套件的可寫的檔案系統。

## 參考資料
* [[CentOS 7] 檔案傳輸界的隱形冠軍 - TFTP 伺服器](http://blog.itist.tw/2016/09/install-a-tftp-server-on-centos-7.html)
## 實驗一
```
virtual machine:

sudo yum install -y xinted tftp-server
sudo vim /etc/xinetd.d/tftp
:
server_args = -c -s /var/lib/tftpboot
diable      = no
:
sudo chmod 777 /var/lib/tftpboot
```
## 實驗二
* 可直接對序列阜，因為serial一對一
```
# R1
enable
configure terminal
hostname R1
intreface s1/0
ip addr 12.1.1.1 255.255.255.0
no shutdown

# R2
enable
configure terminal
hostname R2
intreface s1/0
ip addr 12.1.1.2 255.255.255.0
no shutdown
interface s1/1
ip addr 23.1.1.2 255.255.255.0
no shutdown

# R3
enable
configure terminal
hostname R3
intreface s1/0
ip addr 23.1.1.3 255.255.255.0
no shutdown

# R1
router ospf 1
network 12.1.1.0 0.0.0.255 area 0

# R2
router ospf 1
network 12.1.1.0 0.0.0.255 area 0
network 23.1.1.0 0.0.0.255 area 0

# R3
router ospf 1
network 23.1.1.0 0.0.0.255 area 0

# R1
ping 12.1.1.2 # 此時應該會 ping 通

#R3
ping 12.1.1.1 # 此時應該會ping 通

```

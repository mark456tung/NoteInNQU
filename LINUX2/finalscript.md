# 安裝
open new virtual machine 
網路介面卡三張
一開始先設定 4c8t 8g 安裝完
掛載硬碟開機安裝在本機
關機後移除
硬碟開機後 su
yum install upgrade
yum install update

# 網路1115
先ifconfig
記住inet netmask broadcast
先ip route show
記住 內定路由ip
systemctl stop NetworkManager
systemctl disable NetworkManager
systemctl start network
cd /etc/sysconfig/network-scripts/
vim 介面卡名稱
```
TYPE=Ethernet
BOOTPROTO=static
NAME=eno16777736
DEVICE=eno16777736
ONBOOT=yes
IPADDR=192.168.202.133
NETMASK=255.255.255.0
GATEWAY=192.168.202.2
```
systemctl restart network

# ssh 1004

server: 
$systemctl start sshd  
client:
$ssh-keygen
$ssh-copy-id root@ip -p 22
ssh root@ip
傳檔
a copy to b
scp src user@ip:dis
copy a folder
scp -r srcfolder user@ip:dis 
a copy from b
scp user@ip:src dis

# samba 1122

samba 

yum install samba samba-client samba-common -y
cd / 
mkdir mysamba
vim /etc/samba/smb.conf
![](smb.conf設定.png)
systemctl restart smb
systemctl status smb
smbpasswd -a root 
![](samba-windows1.png)
![](samba-windows2.png)
登入後下次 登入會免密登入，導致無法切換使用者
要切換使用者 要先在terminal 
打net user * /delete

要讓大家都可以讀取資料夾
把valid users = root 刪掉 
加上 public = yes
restartx

chmod 777 mysamba

考試可能會考:
其中一個資料夾大家都可以存取
另一個資料夾只有tom可以存取

# nfs 1115


getenforce
systemctl status firewalld.service

server
yum install -y nfs-utils

vim /etc/exports
要分享的資料夾 要分享給誰
ex:/mynfs 192.168.56.0(rw,sync,fsid=0)
/mynfs 192.168.56.0(rw,sync,no_root_squash,no_all_squash) 安全性較高 chmod 755
systemctl start rpcbind
systemctl start nfs-server
systemctl status rpcbind
systemctl status nfs-server
rpcinfo -p
exportfs -r
exportfs
若要讓client 可以寫 chmod 777 filepath
systemctl restart rpcbind
systemctl restart nfs-server

client
systemctl start rpcbind
systemctl status rpcbind
showmount -e server端的ip
mount -t nfs server'sip:filePath localpath
解除掛載umount filepath

# 檔案伺服器ftp 1213 (not secure通常只在內網用) (10分upload and download)

要注意clinet端的防火牆有無開放ftp使用
FTP out-of-band 一個port傳commnad若需要傳data會另外再開一個port(20)傳完即關閉
ssh talnet in-band data和commnad 在同一個port

yum install vsftpd -y
systemctl start vsftpd
systemctl status vsfptd

netstat -tunlp | grep 21
用cmd登入
ftp ip
user: anonymous
passwrod:隨便

ftp 在linux上的家目錄為/var/ftp/
cd /var/ftp/
chmod 777 pub
在pub 傳輸檔案
ftp有主動跟被動模式
chroot 改變家目錄的位置 主要是為了安全性
傳檔前先切bin模式
get filename (get file from server)
prompt off (關閉互動)
mget
put
prompt off (關閉互動)
mput


clinet 要有寫的權限才可以上傳到server
vim /etc/vsftpd/vsftpd.conf
把anon_upload_enable=YES 的#拿掉
systemctl restart vsftpd

rpm -qa | grep vsftpd 檢查有無安裝
不讓使用者切到其他目錄
chroot_local_user=YES
allow_writeable_chroot=YES

禁止某個使用者登入
vim /etc/vsftpd/user_list 
把使用者加進去
然後重開vsftpd


# haproxy load balancer hardwer F5 software LVS HAPROXY

vip:vitrual ip  在load balancer上
rip:real ip 在實際的server上
load balancer還有分成l4及l7的
https://jaminzhang.github.io/lb/L4-L7-Load-Balancer-Difference/

pc1 (load balancer)
getenforce
systemctl status firewalld
yum install epel-realease
yum install haproxy -y
cd /etc/haproxy/
cp haproxy.cfg haproxy.cfg.bak 
在原本的cfg貼上
```
global
  daemon
  chroot /var/lib/haproxy
  user haproxy
  group haproxy
  stats timeout 30s

defaults
  mode http
  log global
  option httplog
  option dontlognull
  timeout connect 5000
  timeout client 50000
  timeout server 50000

frontend http_front
  bind *:80
  stats uri /haproxy?stats
  default_backend http_back

backend http_back
  balance roundrobin
  server server_name1 192.168.5.3:80 check 
  server server_name2 192.168.5.4:80 check
```
開啟haproxy後如無法順利連上 有可能是本機上的80port已經被占用
可以用netstat -tunlp | gerp 80 確認
pc2 (server1)
systemctl start httpd
vim /var/www/html/index.html
內容打centos 7-2 192.168.202.130
pc3 (server2)
systemctl start httpd
vim /var/www/html/index.html
內容打centos 7-3

# www 1101

```
apache:
先切su

yum install httpd

systemctl status firewalld 看防火牆關了沒
systemctl disable firewalld
getenforce
如果不是disabled
要修改 /etc/selinux/config
+上SELINUX=disabled 
然後重開機
systemctl start httpd
```
```
mariadb
yum install mariadb-server mariadb
systemctl start mariadb.service
systemctl status mariadb
mysql_secure_installation

mysql -uroot -p

show databases;
create database testdb;
use testdb;
create table addrbook(name varchar(50) not null, phone varchar(10));
insert into addrbook(name,phone) values("tom", "1234567890');
insert into addrbook(name,phone) values("marry", "0987654321");
select * from addrbook;
```
```
php
yum install php php-mysql
systemcrl restart httpd
cd /var/www/html
vim test.php
```
hi.php

```php
#hi.php
<?php
$servername="localhost";
$username="root";
$password="user";
$dbname="testdb";

$conn = new mysqli($servername, $username, $password, $dbname);

if ($conn->connect_error) {
    die("conneciton failed: ". $conn->connect_error);
}
//echo "connection ok"

$sql="select name,phone from addrbook";
$result=$conn->query($sql);

if($result->num_rows>0){
   while($row=$result->fetch_assoc()){
      echo "name:". $row["name"]." phone:". $row["phone"]."<br>";
   }
}
?>
```

# bind 

# telnetd 1206 

telnet 用xinetd代理 (若實做應該有10分)
su
yum install telnet
yum install telnet-server
rpm -qa | grep xinetd <!--check if already install xinetd -->
yum install -y xinetd
cd /etc/xinet.d/
vim telnet

```
service telnet

{
    flags = REUSE
    socket_type = stream
    wait = no
    user = root
    server = /usr/sbin/in.telnetd
    log_on_failure += USERID
    disable = no
}
```
socket_type 
dgram 用udp  
stream 用tcp

啟動
systemctl start xinetd
systemctl status xinetd
netstat -tunlp | grep 23
用putty 選擇telnet 然後要改port:23

ps -ef | grep telnet 可以看telnet是否有啟動

# DHCP 1220
dhcp dora
先在虛擬機 新增網路卡 vm ware 選擇LAN segment
先測試內定路由是否可以ping
server:
```
systemctl stop NertworkManager
ip addr add 192.168.10.1/24 brd + dev ens34
```
clinet:
```
systemctl stop NertworkManager
ifconfig ens33 0
ip addr add 192.168.10.2/24 brd + dev ens33

```
server:
```
yum install dhcp
vim /etc/dhcp/dhcpd.conf

subnet 192.168.10.0 netmask 255.255.255.0 {
    range 192.168.10.100 192.168.10.200;
    option routers 192.168.10.1;
    option domain-name-servers 8.8.8.8;
    default-lease-time 600;
    max-lease-time 7200;
}
systemctl restart dhcpd.service
```
client:
```
ifconfig ens33 0
dhclient ens33
netstat -rn
```



# NAT 

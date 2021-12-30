# 安裝
open new virtual machine
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

# nfs

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
insert into addrbook(name,phone) values ("tom", "1234567890');
insert into addrbook(name,phone) values ("marry", "0987654321");
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

# 
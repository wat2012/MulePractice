# 在华为云安装 Anypoint Platform

## 1. 准备
### 1.1 安装包准备
anypoint-1.6.0-installer.tar  
agent-setup-1.5.2.zip
mule-ee-distribution-standalone-3.8.4.tar.gz
### 1.2 licence 准备
MuleSoft trial license key_chinasoft-poc-on-prem_2 months_20170624.txt
### 1.3 虚拟机准备
操作系统：CentOS 7.3
磁盘准备：
SSD 35G  ext3 
SSD 200G ext4 /var/lib/gravity
SSD 100G ext4 /var/lib/gravity/planet/etcd
SSD 400G ext4 /var/lib/data

## 2. 安装
### 2.1 磁盘安装
使用fdisk初始化磁盘分区  
使用mkfs.ext4格式化磁盘  
`mkfs.ext4 /dev/xvde1`  
对docker分区启用GPT  
`parted /dev/xvde mklabel gpt`  
修改/etc/fstab准备磁盘挂载  
`/dev/xvdb1 /var/lib/gravity 			ext4    defaults        1 2   
/dev/xvdc1 /var/lib/data 				ext4    defaults        1 2  
/dev/xvdd1 /var/lib/gravity/planet/etcd ext4    defaults        1 3  `
使用mount挂载磁盘   
### 2.2 修改SeLinux和防火墙设置  
`setenforce Permissive`  

### 2.2 GUI环境安装   
安装Gnome和VNC以及FireFox浏览器（谷歌浏览器root帐户不能运行）  
`yum groupinstall "GNOME Desktop"  
yum install -y tigervnc-server    
vncpasswd  
`
### 2.3 Anypoint Platform 安装   
解压安装包：  
`tar xvf anypoint-1.6.0-installer.tar `  
进入解压后的目录执行安装命令  
`./install`  
根据安装提示进行安装，如果需要在浏览器打开，请使用VNC客户端连接到服务器的VNC，使用服务器上的浏览器进行安装


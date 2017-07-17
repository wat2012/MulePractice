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
/dev/xvdd1 /var/lib/gravity/planet/etcd ext4    defaults        1 3`  
使用mount挂载磁盘   
### 2.2 修改SeLinux和防火墙设置  
`setenforce Permissive  
firewall-cmd --permanent --direct --add-rule ipv4 filter FORWARD 1 -o docker0 -j ACCEPT -m comment --comment "docker subnet"   
firewall-cmd --permanent --direct --add-rule ipv4 filter FORWARD 1 -s 10.0.0.0/8 -j ACCEPT -m comment --comment "docker subnet"`    

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
### 2.4 配置LoadBalance  
参照[此处](https://docs.mulesoft.com/anypoint-private-cloud/v/1.6/install-create-lb) 不需要设置SSL证书，SSL证书可以在管理界面设置  
### 2.5 安装 runtime  
解压mule-ee-distribution-standalone-3.8.3.zip  
将agent-setup-1.5.2 解压并将其中的文件覆盖到 mule-enterprise-standalone-3.8.4/bin目录下  
需要先配置临时环境变量 `JAVA_TOOL_OPTIONS="-javaagent:$HOME/bin/lenient-verifier.jar" `  
然后再根据添加服务器的命令，参照如下命令 
` ./amc_setup -A https://117.78.36.145/hybrid/api/v1 -W "wss://117.78.36.145:8889/mule" -D https://117.78.36.145/apigateway/ccs -F https://117.78.36.145/apiplatform -C https://117.78.36.145/accounts -H 59415645-cdf9-4832-aeda-ec7203b9efc1---1 你的服务器名称`  

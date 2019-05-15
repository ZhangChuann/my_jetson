映射linux到winows本地磁盘
======================
安装准备
---------------------

把linux服务器上的文件夹映射到本地作为一个磁盘来访问，步骤如下：
sudo apt-get install samba  
sudo apt-get install cifs-utils     
sudo apt-get install samba-common

创建共享目录
---------------------
mkdir /home/john/share    //若配置的共享目录不存在则创建  
sudo chmod 777 -R /home/john/share

创建Samba配置文件
---------------------
保存原有配置  
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak  
在smb.conf最后添加   
[share] G    
path = /home/john/share   
available = yes     
browseable = yes     
public = yes   
writable = yes   
valid users = myname   
create mask = 0700   
directory mask =0700   
force user =nobody  
force group = nogroup   

创建账户密码
---------------------
sudo touch /etc/samba/smbpasswd  //设置远程访问磁盘时的密码　  
sudo smbpasswd -a myname   //myname 是系统账户名。  

service smbd restart //重启 samba  

结果演示
---------------------
<img src="https://github.com/ZhangChuann/my_jetson/raw/master/docs/images/1_jet_instrall_3.png">


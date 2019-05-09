# 1.启动与远程连接

## （1）启动

我是在淘宝上购买的nano，附带64GB的内存卡，已经为我烧写好了ubuntu系统，因此这里按照下图组装完成之后，
直接上电启动，直接进入ubuntu用户设置。

<img src="https://github.com/ZhangChuann/my_jetson/raw/master/docs/images/1_jet_install_1.jpg">

<img src="https://github.com/ZhangChuann/my_jetson/raw/master/docs/images/1_jet_install_2.jpg">

## （2）远程SSH到自己NANO

由于nano本身是ubuntu操作系统，而且其本省的CPU配置也是相对比较低的，可以通过远程操作来实现在自己的电脑上编写程序，在NANO上运行cuda程序，


这样就可以工作娱乐两不耽误。安装一个MobaXterm可以直接实现文件远程查看和Teminal运行；



<img src="https://github.com/ZhangChuann/my_jetson/raw/master/docs/images/1_mobaxterm.jpg">

## (3) 设置Samba远程资源管理

使用MobaXterm可以实现文件的查看与copy，通过配置samba可以向浏览本地文件一样浏览远程文件。

```
sudo apt-get install samba
sudo vi /etc/samba/smb.conf
#add below line to the smb.conf

[nano]
   comment = nano share folder
   browseable = yes
   path = /home/your_name/workspace
   writable = yes
   create mask = 0700
   directory mask = 0700
   admin users = your_name
   valid users = your_name
   force user = nobody
   force group = nogroup
   public = yes
   available = yes

sudo smbpasswd -a your_name
sudo service samba restart

```

现在，你可以通过\\ip,访问你的NANO设备。
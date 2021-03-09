# Yum源的使用方法

```
#####################################
#####################################
####### YUM 使用方法 （仅redhat 6.2) ##########
 #####################################
#####################################



#   在客户机/etc/yum.repos.d/下面新建rhel-media.repo文件
#    rhel-media.repo文件内容如下：

[rhel-media]
name=Red Hat Enterprise Linux 6.2
baseurl=http://1.1.1.1   
#baseurl=file:///media/cdrom     #/media/cdrom为光盘的mountpoint
enabled=1
gpgcheck=0
#gpgkey=file:///media/rhel/RPM-GPG-KEY-redhat-release

#    在命令行下 yum install 应用名  即可安装




#########################################
#####使用本地源#############################
####根据源的不同可分为两种情况

########### 用光驱中的光盘做源
#第一步：  mkdir  /media/cdrom
#第二步：  mount   /dev/sr0   /media/cdrom
#第三步：  把rhel-media.repo文件中baseurl=http://1.1.1.1前加# 
#第四步：  把rhel-media.repo文件中#baseurl=file:///media/cdrom中的#删掉

##########用系统ISO镜像文件做源
#第一步：  mkdir  /media/cdrom
#第二步： mount -o loop   /rhel-server-6.2-x86_64-dvd.iso  /media/cdrom       
###   /rhel-server-6.2-x86_64-dvd.iso为ISO文件路径，根据实际情况更改
#第三步：  把rhel-media.repo文件中baseurl=http://1.1.1.1前加# 
#第四步：  把rhel-media.repo文件中#baseurl=file:///media/cdrom中的#删掉



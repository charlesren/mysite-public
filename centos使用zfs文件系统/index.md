# Centos使用ZFS文件系统

#### 安装epel，zfs仓库
epel可以使用中科院开源软件协会等

mirrors.opencas.cn

keys不用管，安装库里软件时会提示import

#### 安装zfs
```
yum install zfs
```
*kernel-devel等会自动安装*
#### 内核加载zfs模块
提示zfs not found

重启系统（dmks staus显示added，没有安装）

重启后变为installed

modprobe zfs

导入pool

升级pool

正常使用


#### 重启后自动import
crontab 加如下内容
```
@reboot sleep 30；/sbin/zpool import data
```

```
zfs get sharenfs
systemctl start rpcbind
systemctl enable nfs
systemctl start nfs
```
#### 共享设置
vi /etc/exports

/data/Downloads    192.168.2.0/24（rw，no_root_squash，sync）后来又加上insecure 不加macmount不上

#### mac上远程挂载
```
mount -t nfs 192.168.2.215/data/Downloads /data/Downloads
```

zfs set sharesmb=on data

zfs建文件系统时要加casesensitivity=mixed属性  与windows共享方便阅览



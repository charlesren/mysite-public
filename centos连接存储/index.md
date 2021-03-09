# Centos连接存储

### device-mapper
**检查是否安装多路径软件包  device-mapper  device-mapper-multipath**
```
rpm -qa |grep device-mapper
```
			

**安装device-mapper, device-mapper-multipath**
```
yum install -y    device-mapper  device-mapper-multipath
```

**生成多路径配置文件**
```
mpathconf --enable
```
*说明：生成配置文件是/etc/multipath.conf*

**修改配置文件**
```
vim /etc/multipath.conf
#blacklist {
#devnode "*"
#}
defaults {
user_friendly_names yes
path_grouping_policy multibus
failback immediate
no_path_retry fail
}
```
> ```
> 在默认配置下，LVM对所有磁盘进行扫描，以确定哪一块上面包含PV。如果使用了device-mapper-multipath或其他多路径软件如 EMC PowerPath 或Hitachi Dynamic Link Manager (HDLM)，那么对于每一个LUN的每一条路径就注册成为一个不同的SCSI设备。如/dev/sdb， /dev/sdc，等等。多路径软件之后会创建一个新的设备映射到这些路径如：/dev/mapper/mpath1 (device-mapper-multipath)， /dev/emcpowerb (EMC PowerPath)，或 /dev/sddlmab (HDLM)。由于每一个设备都指向同一个LUN，所以它们都包含相同的LVM元数据，因此在扫描时显示为duplicate。运行任何LVM命令，都会出现你所说的告警。
> 
> With a default configuration, LVM commands will scan for devices in /dev, and check every resulting device for LVM metadata.   This is caused by the default filter in /etc/lvm/lvm.conf:
> 
>  
> 要确保LVM只扫描想要的多路径设备而不是单条路径，需要在/etc/lvm/lvm.conf文件中加上过滤条件，格式如下：
> filter = [ "a/.*/" ]
> 过滤语法接收多重条件。形式如 "a" "" (add) or "r" "" (remove)，中间以逗号为间隔。所有符合add条件的设备都会被扫描，同时忽略所有符合remove条件的设备。
>  
> 注意：如果本地存储设备包含物理卷，确保除了多路径设备外，这些设备也包含在过滤条件中。
>  filter = [ "a/emcpower.*/","a/mpath.*/","r/sd.*/" ]
>  
> 然后重新扫描一下确认所有设备都能被看见：
> #pvscan
> #vgscan
>  
> 注意如果根文件系统位于逻辑卷上，确保上述扫描命令列出卷组中所有物理卷。如果没有列出，不要重启系统，直到过滤条件修改正确为止。
>  
> 过滤条件修改好后，建议重新编译initrd以使所需设备在重启后能够自动扫描。
> 补充：
> 使用上述方法仍然没有解决问题，最终问题通过如下方法解决
> 1，修改了/etc/fstab 文件，文件系统为延时mount，也就是powerpath启动后在mount
> 
> /dev/dbvg/lv_mysql      /data                     ext3    _netdev        1 2                 
> 
> 2，mysql最后再启动，防止文件系统没有mount就启动了
> lrwxrwxrwx 1 root root 15 Mar  2  2013 S99mysql -> ../init.d/mysql
> ```


**启动multipath服务**
```
modprobe dm-multipath
/etc/init.d/multipathd start
chkconfig multipathd on
```

**查看磁盘**
```
multipath -ll
```

### LVM配置

> 使用/dev/mapper/下带wwid的盘
>
> 不要用/dev/dm-0 之类的盘   
>
> dm-0.... 内部使用

**创建vg,lv**
```
pvcreate /dev/mapper/360050768018207840000000000000000
pvcreate /dev/mapper/360050768018207840000000000000001
vgcreate vg_data /dev/mapper/360050768018207840000000000000000 /dev/mapper/360050768018207840000000000000001
lvcreate -L 600G -n lv_data vg_data
mkfs.ext4 /dev/vg_data/lv_data
mkdir /data
chmod 755 /data
mount /dev/vg_data/lv_data /data
```

**开机自动挂载文件系统**

/etc/fstab 文件添加如下内容
```
/dev/vg_data/lv_data   /data   auto  defaults  0  0
```
**调整LV大小**
- 扩大LV
  ```
  lvextend -L +350G /dev/mapper/vg_data-lv_data
  ```
- 缩减lv
  ```
  lvreduce -L -20G  /dev/mapper/vg_blade4linux-lv_home
  ```

**调整文件系统大小**
- ext4
  ```
  resize2fs   /dev/mapper/lv_data-lv_data
  ```
- xfs
  ```
  xfs_growfs  /dev/mapper/lv_data-lv_data
  ```

**删除vg**
```
umount /u01
lvremove /dev/mapper/vg_data-lv_u01
vgs
pvs
pvremove /dev/sdb
vgreduce vg_data /dev/sdb
vgremove vg_data
```


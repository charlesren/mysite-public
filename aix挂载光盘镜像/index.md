# Aix挂载光盘镜像

AIX 6.1 TL 4开始，AIX系统提供了一个loopmount命令，可直接mount ISO文件

挂载点是目录，不要用建立的文件系统做挂载点

挂镜像文件:
```
loopmount -i PowerHA6.1.iso -o "-V cdrfs -o ro" -m /mnt
```

挂光驱：
```
mount -rv cdrfs /dev/cd0 /mnt
```





**AIX 6.1 TL4之前系统步骤示例如下：**

一：建立一个和iso文件差不多大小的裸设备。
```
mklv -y is0_lv datavg 10
```

二：用dd命令把iso文件写到这个裸设备上，
```
dd if=test_unix_mount_iso.iso of=/dev/iso_lv bs=10M
```

三：mount 
```
mount -rv cdrfs /dev/iso_lv   /mnt
```

这样，在mnt下就是iso文件的内容了。



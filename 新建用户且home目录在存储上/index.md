# 新建用户且home目录在存储上

```
groupadd -g 1100 xprice
useradd -u 1101 -g 1100  -d /tmp/xprice xprice

umount /data

mkdir /home/xprice
mount -a

chown xprice:xprice /home/xprice
chmod 750 /home/xprice
usermod -m  -d /home/xprice xprice
cp /tmp/xprice/.* /home/xprice/
passwd xprice

或可尝试
groupadd -g 1100 xprice
useradd -u 1101 -g 1100 -M  xprice

umount /data

mkdir /home/xprice
mount -a

chown xprice:xprice /home/xprice
chmod 750 /home/xprice
usermod -d /home/xprice xprice
passwd xprice

未验证如下tips
http://blog.sina.com.cn/s/blog_8d778f9d0102wnmb.html
```



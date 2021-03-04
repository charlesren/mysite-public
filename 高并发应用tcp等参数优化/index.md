# 高并发应用TCP等参数优化

```
cp  /etc/sysctl.conf  /etc/sysctl.conf.bak
vim /etc/sysctl.conf
```
> net.ipv4.tcp_tw_reuse = 1
>
> net.ipv4.tcp_fin_timeout = 30
>
> net.ipv4.ip_local_port_range = 1024 65000
>
> net.ipv4.tcp_max_tw_buckets = 45000

```
sysctl  -p  /etc/sysctl.conf
sysctl -w net.ipv4.route.flush=1
sysctl  -a  |grep  net.ipv4.tcp_tw_reuse
sysctl  -a  |grep  net.ipv4.tcp_fin_timeout
sysctl  -a  |grep  net.ipv4.tcp_max_tw_buckets
sysctl  -a  |grep  net.ipv4.ip_local_port_range
```

```
cp  /etc/security/limits.conf  /etc/security/limits.conf.bak
vim /etc/securiy/limits.conf
```

> * soft nofile 65530
>
> * hard nofile 65530


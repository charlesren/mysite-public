# Nginx+keepalived实现apiserver高可用

cat /etc/keepalived/keepalived.conf
```
global_defs {
   router_id nginx_apiserver__test
}

vrrp_script chk_nginx {
        script "/etc/keepalived/check_nginx.sh"
        interval 5
        weight 2
}

vrrp_instance VI_1 {
    state  BACKUP
    interface ens192
    virtual_router_id 51
    priority 99
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        1.1.1.1/24
    }
}
```



check_nginx.sh 脚本内容
```
#!/bin/bash
A=`ps -C nginx --no-header |wc -l`
if [ $A -eq 0 ];then
systemctl  start nginx #重启nginx
if [ `ps -C nginx --no-header |wc -l` -eq 0 ];then #nginx重启失败
exit 1
else
exit 0
fi
else
exit 0
fi
```


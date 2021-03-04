# SSH免密登录

1.1.1.1主机名porxy

1.1.1.9主机名app2

#### 1.生成及验证密钥

Linux keygen设置

Linux默认密钥在/root/.ssh/id_rsa  
```
ssh-keygen -t rsa -P ''  -f /root/.ssh/id_rsa
```
测试本机免秘钥登录

Linux受信任的主机秘钥在 /root/.ssh/authorized_keys
```
[root@porxy .ssh]# ls
id_rsa  id_rsa.pub
[root@porxy .ssh]# cat /root/.ssh/id_rsa.pub >>authorized_keys
[root@porxy .ssh]# ls -al
总用量 16
drwx------   2 root root   58 11月  1 15:48 .
dr-xr-x---. 16 root root 4096 11月  1 15:36 ..
-rw-r--r--   1 root root  392 11月  1 15:48 authorized_keys
-rw-------   1 root root 1679 11月  1 15:36 id_rsa
-rw-r--r--   1 root root  392 11月  1 15:36 id_rsa.pub
[root@porxy .ssh]# chmod 600 authorized_keys
[root@porxy .ssh]# ls -al
总用量 16
drwx------   2 root root   58 11月  1 15:48 .
dr-xr-x---. 16 root root 4096 11月  1 15:36 ..
-rw-------   1 root root  392 11月  1 15:48 authorized_keys
-rw-------   1 root root 1679 11月  1 15:36 id_rsa
-rw-r--r--   1 root root  392 11月  1 15:36 id_rsa.pub
[root@porxy .ssh]#
[root@porxy .ssh]#
[root@porxy .ssh]# ssh 127.0.0.1
The authenticity of host '127.0.0.1 (127.0.0.1)' can't be established.
ECDSA key fingerprint is 32:fe:bf:34:76:32:b1:e7:49:d3:f7:0c:04:d8:d4:e3.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '127.0.0.1' (ECDSA) to the list of known hosts.
Last login: Wed Nov  1 14:43:26 2017 from 10.192.43.98
[root@porxy ~]#
[root@porxy ~]# exit
登出
Connection to 127.0.0.1 closed.
[root@porxy .ssh]#
[root@porxy .ssh]#
[root@porxy .ssh]#
```

可以免秘钥登录了

第一次登录会把ECDSA登记到known hosts文件里
```
[root@porxy .ssh]# ls -al
总用量 20
drwx------   2 root root   76 11月  1 15:51 .
dr-xr-x---. 16 root root 4096 11月  1 15:36 ..
-rw-------   1 root root  392 11月  1 15:48 authorized_keys
-rw-------   1 root root 1679 11月  1 15:36 id_rsa
-rw-r--r--   1 root root  392 11月  1 15:36 id_rsa.pub
-rw-r--r--   1 root root  171 11月  1 15:51 known_hosts
[root@porxy .ssh]#
[root@porxy .ssh]#
```
可以发现/root/.ssh下多了一个known hosts文件 

#### 2.用ssh-copy-id将公钥复制到远程机器中
```
ssh-copy-id -i /root/.ssh/id_rsa.pub 1.1.1.9
```
可以发现远程机器1.1.1.9 上/root/.ssh下多了一个文件authorized_keys

```
[root@app2 .ssh]# ls -al
总用量 8
drwx------   2 root root   28 11月  1 16:12 .
dr-xr-x---. 16 root root 4096 11月  1 16:11 ..
-rw-------   1 root root  392 11月  1 16:12 authorized_keys
[root@app2 .ssh]#
[root@app2 .ssh]# cat authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDFor6qtI7TzYM0TOju3w8EMiyilxJwvYX5s3hYykYaixV2Vhh31g+CdF/E/1kQYZW5oC/gnhSD13sjK87Hymyb1RDGLvUyFgDRIhCRzlNmdE187EiMtch7JLVmb00BXhSEIK7JozGhcy+b/FEHP5zUuVp8RCftZBLRGOqoG1S799bdlujaFIX/zTNHu/SogsYqu/IPilZPrYK3JmG5+KxiixDdTY13soWOF57qVf9CCpR32nkZ7JeIKGA/uhOlnrnEWezQcU4TfRCitJaAZu9uQbbOE/HOlb9apT8JcvxbqwEGcoVUM+COI84Hk95rVamwuc4pgQL6rYHFqqYq4YO7 root@porxy
[root@app2 .ssh]#
```

#### 3.验证免密钥登录
```
[root@porxy .ssh]# ssh 1.1.1.9
Last login: Wed Nov  1 16:16:36 2017 from 1.1.1.1
[root@app2 ~]#
```


# Centos8安装VNC


Centos8  配置VNC 

### 安装 TigerVNC server

```
dnf install tigervnc-server
```

### 防火墙放行VNC服务

```
firewall-cmd --add-service=vnc-server --permanent

firewall-cmd --reload
```

### 配置VNC 用户

vi  /etc/tigervnc/vncserver.users

添加用户，如：

：1=root

### 配置分辨率

vi  $HOME/.vnc/config

geometry=1920x1080

### 设置VNC用户密码

```
vncpasswd
```

 set “view-only password”, press ‘n’ key

### 启动服务

```
systemctl start vncserver@:1

systemctl status vncserver@:1
```

### 连接到远程设备

vnc viewer 

xxx.xxx.xxx.xxx:1




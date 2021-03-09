# Centos7.6离线安装docker

1、安装环境：

系统：CentOS Linux release 7.6.1810 (Core)

Docker版本：18.09.8

2、下载离线安装包

[docker](https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-18.09.8-3.el7.x86_64.rpm)

依赖包下载：

[containerd.io](https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.2-3.el7.x86_64.rpm)

[docker-ce-cli](https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-cli-18.09.8-3.el7.x86_64.rpm)

[container-selinux](https://pkgs.org/download/container-selinux)

3、安装
```
rpm -ivh docker-ce-cli-18.09.8-3.el7.x86_64.rpm
rpm -ivh container-selinux-2.107-3.el7.noarch.rpm
rpm -ivh containerd.io-1.2.2-3.el7.x86_64.rpm
rpm -ivh docker-ce-18.09.8-3.el7.x86_64.rpm
```


# Gitlab容器化部署

OS: Redhat 7.6

### 0：Docker 安装
需要如下4个安装包。（container-selinux取代老版本docker-selinux）
- docker-ce-18.09.0-3.el7.x86_64.rpm
- containerd.io-1.2.0-3.el7.x86_64.rpm 
- docker-ce-cli-18.09.0-3.el7.x86_64.rpm
- container-selinux-2.74-1.el7.noarch.rpm
### 1.gitlab 镜像准备
外网下载镜像
```
docker save -o  gitlab.tar gitlab:latest
docker load -i gitlab.tar 
```
### 2.建gitlab用户
### 3.持久化存储准备
准备存储挂载到/data下。

新建gitlab/config   gitlab/logs    gitlab/data三个文件夹
### 4.gitlab用户启动容器
```
sudo docker run --detach \
   --hostname gitlab.XXXXXX.cn \
   --publish 443:443 --publish 80:80  --publish 2222:22 \
   --name gitlab \
   --restart always \
   --volume /data/gitlab/config:/etc/gitlab \
   --volume /data/gitlab/logs:/var/log/gitlab \
   --volume /data/gitlab/data:/var/opt/gitlab \
   gitlab/gitlab-ce:latest
```
> 注意
>
> 把官方文档中映射的端口修改一下，解决22端口冲突
>
> --publish 443:443 --publish 80:80  --publish 22:22 \ 改为
>
> --publish 443:443 --publish 80:80  --publish 2222:22 \
### 5.修改gitlab.rb（data/gitlab/config下）相关端口号为2222
gitlab_rails['gitlab_shell_ssh_port'] = 2222



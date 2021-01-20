# Kodi的使用

#### celedhrim版kodi-server
##### centos 系统   
```
docker pull celedhrim/kodi-server   
docker run -d --restart="always" --net=host --privileged -v /tmp/kodi:/opt/kodi-server/share/kodi/portable_data  celedhrim/kodi-server   
```

不加--privileged 容器会不断重启,docker logs显示   
> Could not init logging classes. Log folder error (/opt/kodi-server/share/kodi/portable_data/temp/)
> ERROR: Unable to create application. Exiting

**Attention:** /tmp/kodi换成要保存kodi配置文件的目录




#### linuxserver.io headless版
##### centos系统
有问题上linuxserver.io论坛找;unraid论坛forums.lime-technology.com也有相关的东西;

好处：
- 配置和程序分离，方便升级版本；   
- 库由mysql/mariadb集中管理，每个docker kodi instances都从db里获取信息，库更新后，都能看到一样的东西；
- 在一个客户端操作后，或更新文件后，所有客户端能看到最新状态，比如tv看到多少分钟停下的；  

**Attention:** 不要docker clean library ，通过任意一个clinet做

```
docker pull linuxserver/kodi-headless
mkdir /root/kodi
docker run -d --restart="always" --net=host  --privileged  --name=kodi-headless -v /root/kodi:/config/.kodi -p 8080:8080 -p 9777:9777/udp  -e PGID=0 -e PUID=0  linuxserver/kodi-headless
```
不加--privileged会报错，不能生成配置文件;--name 会给容器命名，好识别，好链接   

第一次运行后会在/root/kodi下生成配置文件，确认生成后停掉kodi-headless    

可以加-e TZ=Asia/Shanghai修改时区，没有尝试   

```
docker stop kodi-headless
```
根据自己情况修改配置文件advancedsettings.xml
```
docker start kodi-headless
```
这时gui还是不能访问,修改 guisettings.xml里addonupdates 0到2,仍不能访问
```
systemctl stop firewalld.service
systemctl disable firewalld.service
```
关闭防火墙后，可以访问了。

##### Mac系统


```
docker pull linuxserver/kodi-headless
docker pull linuxserver/mariadb
mkdir  /Users/rencc/Kodi
docker run  -d --restart="always" --net=host  --privileged  --name=kodi-headless -v /Users/rencc/Kodi:/config/.kodi -p 8080:8080 -p 9777:9777/udp  -e PUID=501 -e PGID=20 -e TZ=Asia/Shanghai  linuxserver/kodi-headless
```
Docker logs containeridxxxxxxxxxxxx可以看到服务已经启动   

ls /Users/rencc/Kodi发现下面已经生成配置文件了

后续可通过如下命令启停
```
docker stop kodi-headless
docker start kodi-headless
```



# 安装emby

docker pull emby/embyserver

```
# cat emby.sh
docker run  -d \
    --name embyserver \
    --volume /root/emby/programdata:/config \
    --volume /data/Entbak:/mnt/Entbak \
    --volume /data/Video:/mnt/Video \
    --volume /data/Moives:/mnt/Moives \
    --volume /data/Pictures:/mnt/Pictures \
    --volume /data/Music:/mnt/Music \
    --volume /data/Photos:/mnt/Photos \
    --volume /data/Private:/mnt/Private \
    --volume /data/TV:/mnt/TV \
    --publish 8096:8096 \
    --publish 8920:8920 \
    --env UID=0 \
    --env GID=0 \
    --env GIDLIST=0 \
    docker.io/emby/embyserver:4.1.0.23
#
```


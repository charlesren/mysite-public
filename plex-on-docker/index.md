# Plex on Docker

```
docker run -d --restart=always  --net=host  --privileged  --name plex   -h rencc -v /Users/rencc/Plex:/config -v /Users/rencc/Downloads:/data -p 32400:32400 -e PUID=501 -e PGID=20 -e TZ=Asia/Shanghai  linuxserver/plex
```


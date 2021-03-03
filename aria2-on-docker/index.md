# Aria2 on Docker

```
[root@192 conf]# cat aria2.sh
#mkdir -p  ~/aria2/conf
#cd ~/aria2/conf
#touch aria2.conf
#touch aria2.session
#touch aria2.session
#touch dht.dat
#touch dht6.dat
docker run -d \
--name aria2-with-webui \
-p 6800:6800 \
-p 6880:80 \
-v /data/Downloads:/data \
-v  ~/aria2/conf:/conf  \
-e PGID=0 \
-e PUID=0 \
abcminiuser/docker-aria2-with-webui:latest-ng
```


```
[root@192 conf]# cat aria2.conf
dir=/data
input-file=/conf/aria2.session
save-session=/conf/aria2.session
ave-session-interval=60

file-allocation=trunc
log-level=warn
enable-http-pipelining=true

max-concurrent-downloads=10
max-connection-per-server=10
min-split-size=10M
split=10
continue=true
max-overall-download-limit=0
max-overall-upload-limit=1K

enable-dht=true
enable-dht6=true
dht-file-path=/conf/dht.dat
dht-file-path6=/conf/dht6.dat

bt-seed-unverified=true
bt-save-metadata=true
bt-max-open-files=16
bt-enable-lpd=true
bt-remove-unselected-file=true
follow-torrent=true

peer-id-prefix=-TR2770-
user-agent=Transmission/2.77

enable-rpc=true
rpc-listen-all=true
rpc-allow-origin-all=true
rpc-listen-port=6800
bt-tracker=udp://tracker.coppersurfer.tk:6969/announce,udp://tracker.open-internet.nl:6969/announce,udp://tracker.leechers-paradise.org:6969/announce,udp://exodus.desync.com:6969/announce,udp://tracker.internetwarriors.net:1337/announce,udp://9.rarbg.to:2710/announce,udp://9.rarbg.me:2710/announce,http://tracker1.itzmx.com:8080/announce,udp://open.demonii.si:1337/announce,udp://tracker.torrent.eu.org:451/announce,udp://tracker.tiny-vps.com:6969/announce,udp://tracker.cyberia.is:6969/announce,udp://thetracker.org:80/announce,udp://denis.stalker.upeer.me:6969/announce,udp://bt.xxx-tracker.com:2710/announce,udp://open.stealth.si:80/announce,udp://tracker.port443.xyz:6969/announce,udp://ipv4.tracker.harry.lu:80/announce,http://open.acgnxtracker.com:80/announce,udp://explodie.org:6969/announce
[root@192 conf]#
```



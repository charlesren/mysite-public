# Nats的使用

单机部署参考[NATS Docker tutorial](https://docs.nats.io/nats-server/nats_docker/nats-docker-tutorial)

集群部署参考[nats_docker](https://docs.nats.io/nats-server/nats_docker)
### 单机部署
####  下载镜像
```
docker pull nats
```
#### 启动服务
```
docker run -p 4222:4222 -p 8222:8222 -p 6222:6222 --name nats-server -d nats:latest
```
#### 查看服务
通过 <http://localhost:8222> 查看服务状态





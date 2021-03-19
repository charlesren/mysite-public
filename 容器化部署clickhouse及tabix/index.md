# 容器化部署Clickhouse及Tabix

#### 下载镜像
```
docker pull yandex/clickhouse-server
```
#### 准备数据目录
> clickhouse server 重要目录介绍
>
> 数据文件: **/var/lib/clickhouse**
>
> 配置文件: **/etc/clickhouse-sever**
>
> 日志文件: **/var/log/clickhouse-server**

以/data为外接存储为例,新建数据目录
```
mkdir -p /data/clickhouse
```

#### 启动clickhouse数据库
```
docker run -d --name clickhouse-server \
--ulimit nofile=262144:262144 \
--volume=/data/clickhouse:/var/lib/clickhouse \
-p 8123:8123 \
-p 9000:9000 \
-p 9009:9009 \
yandex/clickhouse-server 
```
#### 新建数据库用户
> clickhouse默认账号为default,密码为空
>
> 本示例新建名为clickhouse的用户
>
> 容器内无vim命令

##### 准备用户配置文件模板
```
docker cp clickhouse-server:/etc/clickhouse-server/users.xml users.xml
```
##### 准备SHA256加密的密码
命令格式如下
```
PASSWORD=$(base64 < /dev/urandom | head -c8); echo "$PASSWORD"; echo -n "$PASSWORD" | sha256sum | tr -d '-'
```
**注意替换密码，如"123456" 替换 "$PASSWORD"**

执行后，会输出密码明文及SHA256密码串
```
PASSWORD=$(base64 < /dev/urandom | head -c8); echo "123456"; echo -n "123456" | sha256sum | tr -d '-'
123456
8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
```
##### 修改用户配置文件
###### 创建新用户置文件
```
cp users.xml clickhouse.xml
```
###### 修改用户名
修改users.xml的yandex-->users-->default 节点

把节点名称由default替换为clickhouse
###### 修改用户密码
修改users.xml的yandex-->users-->clickhouse-->password 节点

由password替换为password_sha256_hex,并填入SHA256密码串. 如:

```
<password_sha256_hex>8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92</password_sha256_hex>
```
##### 上传用户配置文件到容器
> 新用户的配置文件放到容器中/etc/clickhouse-server/users.d/下
```
docker cp   clickhouse.xml   clickhouse-server:/etc/clickhouse-server/users.d/clickhouse.xml 
```
*注意文件权限*
#### 新用户登录测试
```
docker exec -it clickhouse-server /bin/bash
clickhouse-client -u clickhouse --password 123456
```
收到类似如下提示意即为成功
```
ClickHouse client version 21.1.2.15 (official build).
Connecting to localhost:9000 as user clickhouse.
Connected to ClickHouse server version 21.1.2 revision 54443.
```

#### 安装Tabix 
```
docker run -d -p 8080:80 spoonest/clickhouse-tabix-web-client
```
#### 访问Tabix UI
浏览器打开http://localhost:8080/

*根据实际情况把localhost替换成实际IP*


DIRECT CH tab页输入相关参数即可连接



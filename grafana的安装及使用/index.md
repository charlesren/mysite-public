# Grafana的安装及使用

### 安装 
```
docker pull grafana/grafana 
```
### 准备存储空间
```
mkdir -p /data/grafana
```
### 启动
```
docker run -d -p 3000:3000 -v /data/grafana:/var/lib/grafana --name grafana grafana/grafana
```
### 登录
浏览器访问<http://localhost:3000/login>

> 默认用户名密码为: admin/admin
> 
> 参考[Configure a Grafana Docker image](https://grafana.com/docs/grafana/latest/administration/configure-docker/)
### 安装ClickHouse plugins
> 参考<https://grafana.com/grafana/plugins/vertamedia-clickhouse-datasource/>
```
docker exec -it  grafana /bin/bash
grafana-cli plugins install vertamedia-clickhouse-datasource
exit
docker restart grafana
```
### 配置数据源
Configuration > Data Sources > Add data source 
> - 输入URL
>
> - Access选择Server(default)。
>
> - Auth选择Basic auth
> 
>    在Basic Auth Details 中输入 User 和 Password。
> - Additional
>
>   根据情况设置Default database ,如默认则无需设置
### 自定义Dashboard
Create > Dashboard > Add new panl菜单,编辑Query tab页

- 选择数据源
- Form 选择database名称及表名
- Format As 选择展示方式：时间序列或表格
- Text edit modle 输入SQL
  > 以展示数据库中两列值trade_date,high为例
  >
  > trade_date格式为20200319,high格式为61.64
  >
  > *Format As: Time Series*
  >
  > *库/表  ： tushare/dailies*
  > ```
  > SELECT
  >     (intDiv(toUInt32(toDateTime(concat(substr(trade_date, 1, 4), '-', substr(trade_date, 5, 2), '-', substr(trade_date, 7, 2), ' ', '00:00:00'))), 2) * 2) * 1000 as t,
  >     high
  > FROM $table
  > 
  > WHERE ts_code = '689009.SH'
  > 
  > ORDER BY t
  > ```
- Query inspector 验证sql及数据
- Apply 更改
### 导入Dashboard
Dashboards > manage > Import 页 输入grafana.com dashborad url 或者id
> <https://grafana.com/grafana/dashboards>


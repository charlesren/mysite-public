# R7000刷merlin固件

```
原厂
先刷中间R7000_378.52-2.chk
恢复出厂
dual wan
load banlance 3:1 较1:1网速快
ss
全局代理  全局tcp才能连上选全局https连不上
全局模式下网速慢
 用gfwlist模式，都默认配置



更新7.4
拔掉电信猫到路由器网线，防恢复出厂后地址冲突，自动重启改ip
格式化jffs （ administration-system-format jffs at nest boot）
恢复出厂
加载7.4文件
插网线
ip和电信猫冲突，自动改为192.168.2.1
改路由器管理秘密，无线密码
software center 下载 shadowshocks
enable jffs custom
设置ss（注意勾选使用ssr）
导出ss设置
保持路由器配置文件
```



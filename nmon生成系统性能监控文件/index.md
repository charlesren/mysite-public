# Nmon生成系统性能监控文件

**用nmon命令生成系统性能监控文件**

为了查看系统在一段时间内的性能，我们可以借助系统自身提供的性能监控命令来生成一份性能监控日志，然后用nmon analyser小软件来生成图形化的监控界面.

在aix操作系统下，用root 用户执行如下命令：
```
nmon  -f  -s 30 –c 2880  -m /tmp
```
*系统每隔30秒，总共2880次监控性能，生成的文件存放在/tmp目录下。*



**topas**

系统自动保存topas文件，可以转为csv文件，用nmon工具分析。文件在/etc/perf/daily目录下

topas文件可通过topasout -a xxxxx.topas 转化为csv文件


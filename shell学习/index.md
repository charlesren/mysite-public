# Shell学习

# 变量嵌套
```
[root@renccblj ~]# a=b
[root@renccblj ~]# c=a
[root@renccblj ~]# echo ${!c}
b
[root@renccblj ~]#
[root@renccblj ~]# eval echo $"${c}"
a
[root@renccblj ~]#
```
Aix下只能用 eval


# 算术精度
取小数点后两位
```
[root@renccblj ~]# echo "scale=2; 5/3 " | bc
1.66
[root@renccblj ~]#
[root@renccblj ~]# echo "scale=2; 1/3 " | bc
.33
[root@renccblj ~]#
```

发现零点几前面的0不显示，解决方法如下

http://www.361way.com/linux-bc-point-zero/4960.html
```
#!/bin/bash
#方法1
res1=$(printf "%.2f" `echo "scale=2;1/3"|bc`)
res2=$(printf "%.2f" `echo "scale=2;5/3"|bc`)
#方法2
#v=$(echo $big $small | awk '{ printf "%0.2f\n" ,$1/$2}')
v1=$(echo 1 3 | awk '{ printf "%0.2f\n" ,$1/$2}')
v2=$(echo 5 3 | awk '{ printf "%0.2f\n" ,$1/$2}')
#方法3 
mem1=`echo "scale=2; a=1/3; if (length(a)==scale(a)) print 0;print a "|bc`
mem2=`echo "scale=2; a=5/3; if (length(a)==scale(a)) print 0;print a "|bc`
echo res1 is $res1
echo res2 is $res2
echo v1 is $v1
echo v2 is $v2
echo mem1 is $mem1
echo mem2 is $mem2
```

三种方法我们可以看到，方法1、方法3对小数点后面的值不会四舍五入，而方法2(awk)方法使用printf 时会对小数点（浮点运算）的值四舍五入进位。所以浮点运行时还是建议使用awk处理。不过在取整数时，awk默认也是不会四舍五入的。


# tail 
tail命令语法为：

     tail [ -f ] [ -c Number | -n Number | -m Number | -b Number | -k Number ] [ File ]

     To Display Lines in Reverse Order

     tail [ -r ] [ -n Number ] [ File ]



tail 命令从指定点开始将文件写到标准输出.

number变量指定多少units写到标准输出。注意是units，不是行。

number的值可以是 positive or negative integer。如果值是+n,从文件开始算起，第n个units及以后的内容将被输出到标准输出。如果值是-n,从文件结尾算起，第n个units及以后的内容将被输出到标准输出。如果值是n，不带+或—，等同于-n。

如果加-n 参数，则units为行。

如果没有加任何参数，tail将读取文件最后10行并写到标准输出，相当于tail -n 10


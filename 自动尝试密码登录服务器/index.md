# 自动尝试密码登录服务器

# 方法一：Autologin脚本
适用于密码为一组密码中的某一个

cat /usr/local/bin/Autologin
```
#!/usr/bin/expect -f
##########################################################
# 通过SSH登陆和执行命令
#参数:1.LoginUser
#     2.RemoteServerIp
#返回值：
#     0  成功
#     1  参数个数不正确
#     2  SSH 服务器服务没有打开
#     3  SSH 用户密码不正确
#     4  连接SSH服务器超时
#     5  Unexpected Error
##########################################################
set PassNumber 0
set passwords {123 abc root}
set timeout 20
set TryAgain 1
set ok_string LOGIN_SUCCESS
set RemoteServerIp [lindex $argv 0]
while {$TryAgain == 1}  {
spawn ssh -l root  ${RemoteServerIp}
expect {
    -nocase -re "yes/no" {
        send -- "yes\n"
        exp_continue
    }	
    -nocase -re "assword: " {
        if { $PassNumber >= [llength $passwords] } {
            send_error "wrong passwords\n"
            set CheckLoginState 0
            exit 3
        }
        set TryWhich [expr $PassNumber + 1]
        send [lindex $passwords $PassNumber]\r	
        send_user "\nTry Password $TryWhich ..."
        set CheckLoginState 1
        incr PassNumber
    }
    -nocase -re "Connection refused" {
        send_error "SSH services at ${RemoteServerIp} is not active.\n"
        exit 2
    }
    -nocase -re "ssh_exchange_identification: Connection closed by remote host" {
     #set PassNumber [expr $PassNumber - 1]
     sleep 5
    }
    timeout {
        send_error "Connect to SSH server root@${RemoteServerIp} timeout.\n"
        exit 4
    }	
    eof { 
        send_error "Unexpected Error\n"
        exit 5
    }
}
#######check login state########
if {$CheckLoginState==1} {
    expect {
        -nocase -re "try again" {
            send_error "\nPassword $TryWhich Error.\n\n"
        }
        -nocase -re "assword:" {
            send_error "Password $TryWhich Error.\n\n"
        }
        -nocase -re "ssh_exchange_identification: Connection closed by remote host" {
            send_error "Connection closed by remote host.\n\n"
        }
		
        -nocase -re "Last login: " { 
		set TryAgain 0
        interact
        }

        eof {}
    }
}
#-nocase -re "No such user" {
#        send_error "No such user.\n"
#        exit 5
#    }
#exit
}
```
根据实际情况替换脚本第15行中123 abc root等密码

# 方法二： gansible工具
速度更快，功能更强大

详见[gansible](https://github.com/charlesren/gansible)项目主页


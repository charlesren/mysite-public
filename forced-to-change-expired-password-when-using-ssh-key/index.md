# Forced to Change Expired Password When Using Ssh Key


>I am working in an environment where I have an account on multiple linux machines where accounts and passwords are managed independently (no active directory/LDAP/etc) and passwords expire every 30 days. As such, I thought it would be easier to manage my authentication using ssh keys.   
>
>I am able to authenticate using my ssh keys just fine. However, I found that when my password expires, I am prompted to change my password when I try to connect using my ssh key.   
>
>Is this normal behavior? I thought the whole point of using key pairs is to bypass using your password. Shouldn't I only be prompted to change my password if I login using a password?

Linux服务器用户的密码有过期设置，需定期修改密码。   
凭直觉，可以通过秘钥登录，避免密码过期后，必须修改密码才能进行其他操作,同时也利于编写自动化脚本。  
然而,当密码过期后，通过秘钥登录时，也会提示修改密码。  
那么问题来了。   
这正常吗？使用秘钥登录不是绕过密码?   
如果正常，怎么避免使用key登录还提示修改密码？ 

答案是正常的。可以通过修改[Linux-PAM](http://www.linux-pam.org/Linux-PAM-html/Linux-PAM_SAG.html)及[pam_unix](http://www.linux-pam.org/Linux-PAM-html/sag-pam_unix.html)配置解决。  

**解决方案：**  
修改/etc/pam.d/sshd文件。  
老配置：  
```
account    required     pam_nologin.so
account    include      password-auth
password   include      password-auth
```
新配置：  

```
account    required     pam_nologin.so
account    sufficient  pam_unix.so no_pass_expiry
account    include      password-auth
password   sufficient  pam_unix.so no_pass_expiry
password   include      password-auth
```

**Reference:**  
[Forced to change expired password when using ssh key](https://serverfault.com/questions/733955/forced-to-change-expired-password-when-using-ssh-key)  
[Expired password and SSH key based login with UsePAM yes](https://unix.stackexchange.com/questions/160268/expired-password-and-ssh-key-based-login-with-usepam-yes/368303)  
[PAM 教程](https://www.jianshu.com/p/342c05b51b7c)


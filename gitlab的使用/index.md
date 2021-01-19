# Gitlab的使用


#### 0.把用户是公钥可以导入gitlab；hosts文件加入git服务器解析

#### 1.在需要保存仓库的父目录下

git init

git remote

git remote add origin ssh://[git@xxx.xxx.xxx](mailto:git@zdhgitlab.zzbank.cn):2222/xxx/xxxj.git

***git remote set-url origin ssh://[git@xxx.xxx.xxx](mailto:git@zdhgitlab.zzbank.cn):2222/xxx/xxxj.git

git remote -v

git config --list

git config --global user.email [xxx@xxx](mailto:ren.chuancheng@zzbank.cn)

git config --global user.name xxx

git config --list

git config --global http.sslVerify false

git config --global http.postBuffer 1048576000

git clone ssh://[git@xxx.xxx.xxx](mailto:git@zdhgitlab.zzbank.cn):2222/xxx/xxxj.git

git add .

git status 

git commit -m  &quot;comment&quot;

git   push  origin master

#### 2.推送到码云

git remote add gitee https：//gitee.com/xxx/xxx.git


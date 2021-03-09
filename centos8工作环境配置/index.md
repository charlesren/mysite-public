# Centos8工作环境配置

# 配置网络

#### 查看并连接设备
nmcli d 

nmcli d   connect xxx

#### 查看并配置连接
nmcli c

nmcli c modify ens192 ipv4.address x.x.x.x/24 ipv4.gateway x.x.x.x ipv4.dns 8.8.8.8 ipv4.method manual  connection.autoconnect yes

> 114.114.114.114 cat not resolve gcr.io etc, but 8.8.8.8 can

#### 启用连接
nmcli c reload

nmcli c up xxx

### Centos8 源设置
```
dnf config-manager --add-repo https://mirrors.aliyun.com/repo/Centos-8.repo
```



# 安装docker

#### 安装docker
```
dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
dnf repolist -v
dnf install -y docker-ce  --nobest --allowerasing
```

#### 设置国内源
vim  /etc/docker/daemon.json
```
{
    "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn/","https://hub-mirror.c.163.com"]
}

```
docker info

#### Run docker without sudo
useradd xxx

passwd xxx

usermod -aG docker xxx && newgrp docker

#### 开机启动docker

sudo systemctl enable --now docker

# 安装Golang
#### 卸载旧版本
rm -rf /usr/local/go
#### 安装新版本
官网下载安装包

tar -C /usr/local -xzf gox.xx.x.linux-amd64.tar.gz
#### 设置环境变量
vim $HOME/.bash_profile

export PATH=$PATH:/usr/local/go/bin
#### 设置代理
打开Terminal执行
```
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```

### 安装vnc
参考[Centos8安装VNC](https://charlesren.github.io/mysite-public/centos8%E5%AE%89%E8%A3%85vnc/)

### 安装git  
#### 安装git包
dnf install git
#### 配置ssh key
if [ ! -e /root/.ssh/id_rsa ];then
        ssh-keygen -t rsa -P '' -f /root/.ssh/id_rsa
fi 
#### 把公钥添加到 github(setting>ssh and gpg keys>new ssh key)
#### 测试连接

ssh -T git@github.com

# 安装Hugo

参考[hugo的使用](https://charlesren.github.io/mysite-public/hugo%E7%9A%84%E4%BD%BF%E7%94%A8/)

# 本地k8s集群配置

####  安装kubectl 

```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
echo "$(<kubectl.sha256) kubectl" | sha256sum --check
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

#### 下载特定版本kind
```
curl -Lo ./kind "https://kind.sigs.k8s.io/dl/v0.10.0/kind-$(uname)-amd64"
chmod +x ./kind
mv ./kind /usr/local/bin/kind
```

#### 创建三worker节点k8s集群
vi kind.yaml
```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
# One control plane node and three "workers".
#
# While these will not add more real compute capacity and
# have limited isolation, this can be useful for testing
# rolling updates etc.
#
# The API-server and other control plane components will be
# on the control-plane node.
#
# You probably don't need this unless you are testing Kubernetes itself.
nodes:
- role: control-plane
- role: worker
- role: worker
- role: worker
```

kind create cluster --config=kind.yaml --name yugabyte


#### 安装Helm
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

# Istio配置
#### Install istioctl
```
curl -sL https://istio.io/downloadIstioctl | sh -
export PATH=$PATH:$HOME/.istioctl/bin
```
#### Install istio
```
kubectl create ns istio-system
kubectl apply -f - <<EOF
apiVersion: install.istio.io/v1alpha1
kind: IstioOperator
metadata:
  namespace: istio-system
  name: example-istiocontrolplane
spec:
  profile: demo
EOF
```
#### Confirm the Istio control plane services
```
kubectl get svc -n istio-system
kubectl get pods -n istio-system
```

# Knative配置
#### Installing the Knative Operator
```
kubectl apply -f https://github.com/knative/operator/releases/download/v0.20.0/operator.yaml
```
#### Verify the operator installation
```
kubectl get deployment knative-operator
```
#### Track the log
```
kubectl logs -f deploy/knative-operator
```


# K8s Deploy Yugabyte

#### 添加helm repo
```
helm repo add yugabytedb https://charts.yugabyte.com
helm repo update
helm search repo yugabytedb/yugabyte
```
#### 准备docker image
docker pull yugabytedb/yugabyte:2.5.1.0-b153

**kind 集群需导入镜像**

kind  load yugabytedb/yugabyte:2.5.1.0-b153 --name yugabyte

#### 部署yugabyte
```
kubectl create namespace yb-demo
helm install yb-demo yugabytedb/yugabyte --namespace yb-demo --wait
```

#### Check cluster status with kubectl
```
kubectl --namespace yb-demo get pods
kubectl --namespace yb-demo get services
```

#### Check cluster status with Admin UI
```
kubectl --namespace yb-demo port-forward svc/yb-master-ui 7000:7000
```

####  Run YSQL shell from inside of a tablet server 
kubectl exec --namespace yb-demo -it yb-tserver-0 -- /home/yugabyte/bin/ysqlsh -h yb-tserver-0.yb-tservers.yb-demo

#### Cleanup YugabyteDB Pods
helm delete yb-demo -n yb-demo

>  NOTE: You need to manually delete the persistent volume
>  kubectl delete pvc --namespace yb-demo -l app=yb-master
>  kubectl delete pvc --namespace yb-demo -l app=yb-tserver


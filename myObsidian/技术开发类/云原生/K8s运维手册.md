
1. 优雅关闭集群
```
#先关闭从节点
kubectl drain k8s-node3 --ignore-daemonsets --delete-emptydir-data
kubectl drain k8s-node2 --ignore-daemonsets --delete-emptydir-data
#最后关闭master节点 防止脑裂问题
kubectl drain k8s-node1 --ignore-daemonsets --delete-emptydir-data
```

验证
```
kubectl get pods -o wide | grep <节点名>
```

2. 停止kubectl和docker
```
#所有节点执行
systemctl stop kubelet && systemctl stop containerd
```
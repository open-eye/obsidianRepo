
1. 优雅关闭&恢复集群
```
#优雅关闭
#先关闭从节点
kubectl drain k8s-node3 --ignore-daemonsets --delete-emptydir-data
kubectl drain k8s-node2 --ignore-daemonsets --delete-emptydir-data
#最后关闭master节点 防止脑裂问题
kubectl drain k8s-node1 --ignore-daemonsets --delete-emptydir-data

#所有节点执行
sudo systemctl stop kubelet && sudo systemctl stop docker
```
优雅恢复集群
```
#优雅恢复集群
#所有节点执行
sudo systemctl start docker
sudo systemctl start kubelet

kubectl uncordon k8s-node1
kubectl uncordon k8s-node2
kubectl uncordon k8s-node3
```

验证
```
kubectl get pods -o wide | grep <节点名>
```



2. 从节点not ready
```
#重置 故障节点执行
sudo kubeadm reset --force
#如果节点已经存在 则master执行
kubectl delete node k8s-node
#重新加入
join
```


3. /proc/sys/net/ipv4/ip_forward contents are not set to 1
```
# 临时启用（重启失效） 
sudo sysctl net.ipv4.ip_forward=1 
# 永久生效（需重启节点） 
echo "net.ipv4.ip_forward=1" | sudo tee /etc/sysctl.d/k8s.conf 
sudo sysctl -p /etc/sysctl.d/k8s.conf
```

查看节点状态
```
kubectl describe node k8s-node1
```
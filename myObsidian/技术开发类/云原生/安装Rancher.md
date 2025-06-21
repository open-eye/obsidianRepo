
1. 获取TLS证书（自签名证书）
```
#创建私钥
openssl genrsa -out server.key 2048

#生成证书签名请求 
openssl req -new -key server.key -out server.csr -subj "/C=CN/ST=Beijing/O=DevOps/CN=rancher.com"

#自签名证书
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
```


2. Helm部署高可用Rancher
```
#安装Helm（k8s包管理工具）
curl -fsSL https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

#添加Rancher Helm仓库
helm repo add rancher-stable https://releases.rancher.com/server-charts/stable helm repo update

#创建命名空间与TLS证书
kubectl create namespace cattle-system # 使用上一个步骤的自签名证书（测试）或Let's Encrypt（生产） 
kubectl -n cattle-system create secret tls tls-rancher-ingress \ --cert=server.crt --key=server.key

#使用Helm安装Rancher
helm install rancher rancher-stable/rancher \
--namespace cattle-system \
--set hostname=rancher.com \
--set ingress.tls.source=secret \
--set replicas=3


--set hostname=rancher.com \ # 替换为你的域名 
--set ingress.tls.source=secret \ # 使用预置证书 
--set replicas=3 # 匹配3节点高可用
```

3. 验证Rancher

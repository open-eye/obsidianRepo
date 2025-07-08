1. 网络配置
```
sudo vim /etc/sysconfig/network-scripts/ifcfg-ens33  
# 修改IPADDR=新IP（如192.168.1.101）
sudo systemctl restart network
```
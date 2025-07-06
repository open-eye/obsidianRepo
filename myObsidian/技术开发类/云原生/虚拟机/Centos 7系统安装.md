1. 系统分区（适用于120GB磁盘）
- ​**​`/boot`​**​：200MB（EFI引导分区，XFS文件系统）
- ​**​`/`（根分区）​**​：剩余空间（XFS，存放系统核心文件）
- ​**​`/home`​**​：10GB（可选，用户数据）

2. **系统基础优化​**：
- **禁用Swap​**​：
```
swapoff -a
sed -i '/swap/s/^\(.*\)$/#\1/g' /etc/fstab # 永久禁用
```

- ​**​关闭防火墙与SELinux​**​：
```
systemctl disable --now firewalld
setenforce 0
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

- ​**​内核参数调整​**​（容器网络要求）：
```
cat > /etc/sysctl.d/k8s.conf <<EOF
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF
sysctl -p /etc/sysctl.d/k8s.conf
```

3. ​**​容器运行时配置​**​：
- **​安装Docker​**​（推荐版本20.10+）：
```
yum install -y yum-utils
yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
yum install -y docker-ce
systemctl enable --now docker
```

- **配置docker镜像地址：**
```
# /etc/docker/daemon.json
{
    "registry-mirrors": [
	    "https://9uder41y.mirror.aliyuncs.com",
        "https://docker.m.daocloud.io",
        "https://docker-0.unsee.tech",
        "https://docker.hlmirror.com",
        "https://docker.1ms.run",
        "https://func.ink",
        "https://lispy.org",
        "https://docker.xiaogenban1993.com"
  ]
}
```


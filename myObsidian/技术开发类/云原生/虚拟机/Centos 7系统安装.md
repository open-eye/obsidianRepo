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

- **切换国内源**
```
sudo cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
sudo wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
#清理缓存并重建索引
sudo yum clean all && sudo yum makecache
#验证
yum repolist enabled | grep aliyun
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
sudo mkdir -p /etc/docker && sudo tee /etc/docker/daemon.json <<'EOF'
{
  "registry-mirrors": [
    "https://9uder41y.mirror.aliyuncs.com",
    "https://docker.m.daocloud.io",
    "https://docker-0.unsee.tech",
    "https://docker.hlmirror.com",
    "https://docker.1ms.run",
    "https://func.ink",
    "https://lispy.org",
    "https://docker.xiaogenban1993.com",
    "https://mirror.ccs.tencentyun.com",
    "https://hub-mirror.c.163.com"
  ]
}
EOF

sudo systemctl restart docker
#验证
docker info | grep -A 10 "Registry Mirrors"
```


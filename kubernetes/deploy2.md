

#规划

https://blog.csdn.net/linxi7/article/details/111318769 kubeadm部署安装k8s集群

masterIP 172.28.80.207
node1 172.28.80.207
node2 172.28.80.211

pod 网络  10.18.0.0/16
service 网络 10.1.0.0/16
dns 网络 10.2.0.0/16

 hostnamectl set-hostname k8s_master

#环境准备
```
systemctl stop firewalld && systemctl disable firewalld
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

swapoff -a && sysctl -w vm.swappiness=0

modprobe br_netfilter
cat << EOF | tee /etc/sysctl.d/k8s.conf
net.ipv4.ip_forward = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sysctl -p /etc/sysctl.d/k8s.conf

yum install -y xfsprogs yum-utils device-mapper-persistent-data lvm2
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
sudo yum install docker-ce
sudo systemctl start docker
sudo docker run hello-world

mkdir /etc/docker
cat <<EOF | sudo tee /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2",
  "storage-opts": [
    "overlay2.override_kernel_check=true"
  ]
}
EOF
```

#安装

前提：安装官方文档安装 kubeadmin

```
  kubeadm init --pod-network-cidr=10.18.0.0/16  
  kubeadm token create --print-join-command
```

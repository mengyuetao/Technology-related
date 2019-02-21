#linux 更新到最新版本

#autossh

> https://www.cnblogs.com/fuyanwen/p/8195956.html  centos7 搭建openvpn服务器


```
cd /root
mv id_rsa .ssh
chmod 0600 /root/.ssh/id_rsa

yum install openvpn

yum install make　configure　 gcc
wget http://fossies.org/linux/privat/autossh-1.4e.tgz   使用浏览器下载
tar -xf autossh-1.4f.tar
cd autossh-1.4f
./configure
make
make install
man autossh
autossh -M 61195 -f -N -l root -L127.0.0.1:1194:127.0.0.1:1194   107.191.53.246

openvpn --config 94-s5-1.ovpn

```

# 测试环境网络
1. (添加VPN链路路由)route add  -net  107.191.53.246 netmask 255.255.255.255  gw 172.28.80.1
2. 修改 /etc/resolve.etc 108.61.10.10
3. route add -net 172.28.0.0/16  gw 172.28.80.1



# kubernets初始化步骤

关闭防火墙
关闭swap
配置k8s.conf , modprobe br_netfilter

>https://blog.csdn.net/xiegh2014/article/details/84830880   安装   CentOS7.5 Kubernetes V1.13（最新版）二进制部署集群
https://blog.csdn.net/qq1083062043/article/details/84839609 安装   使用kubeadm安装Kubernetes1.13

# 安装Docker
按照官方文档安装
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

sudo ip link add name docker0 type bridge
sudo ip addr add dev docker0 192.168.0.1/24

# 各节点安装kubeadm和kubelet

见上文
``` systemctl enable kubelet.service
```

#　添加节点

```
kubeadm token list

openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | \
   openssl dgst -sha256 -hex | sed 's/^.* //'

kubeadm join 172.28.80.159:6443 --token ng1n5e.botzjghwgdpzps6r --discovery-token-ca-cert-hash sha256:d85c71ae7359c85bea20602e21714a83797000e69c94b36435816d8f46e71507


```




# dns 安装

# yum 服务

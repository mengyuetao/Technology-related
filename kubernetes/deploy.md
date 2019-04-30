

## 二进制部署集群

安装参考
>CentOS7.5 Kubernetes V1.13（最新版）二进制部署集群
>https://blog.csdn.net/xiegh2014/article/details/84830880

>https://www.cnblogs.com/binchen-china/p/5651142.html
>OpenSSL - 利用OpenSSL自签证书和CA颁发证书

>Centos7五步安装Docker并解决docker官方镜像无法访问问题
>https://blog.csdn.net/doegoo/article/details/80062132

架构理解
> https://www.cnblogs.com/purpleraintear/p/6040067.html
> kubernetes容器编排系统介绍


#### 规划

masterIP 172.28.80.207
node1 172.28.80.207
node2 172.28.80.211

pod 网络  10.18.0.0/16
service 网络 10.1.0.0/16
dns 网络 10.2.0.0/16


#### 环境准备命令

```
systemctl stop firewalld && systemctl disable firewalld
setenforce 0
vi /etc/selinux/config
SELINUX=disabled

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
```


####　创建证书

```
查看证书的命令
openssl x509 -noout -text -in server.pem
openssl req -noout -text -in server.csr
```

ETCD CA 和证书生成

ca-csr.json　生成etcd根CA, 包含CN="etcd CA",算法，名称等信息；文件包含ca.pem　ca_key.pem ca.csr
ca-config.json 其他证书配置信息，包括的有效期、类型
server-csr.json　，包含CN="etcd",服务器IP地址列表，算法，名称等信息; 输入ca-config.json,使用etcd根证书ca.pem，跟证书密钥ca_key.pem，
                    生成etcd服务器证书密钥server_key.pem，服务器证书server.pem(已签名，包含ca信息) ，server.csr服务器证书签名请求文件（无用）

Kubernetes CA 和证书生成(同上)
ca-csr.json
ca-config.json
server-csr.json　　API Server 证书
kube-proxy-csr.json kube-proxy 证书



#### 安装ETCD

补坑(etcd.service etcd.conf)
>https://blog.csdn.net/ShouTouDeXingFu/article/details/81167302 etcd 搭建与使用

```
服务配置,需要创建下面的文件
/usr/lib/systemd/system/etcd.service
/K8S/etcd/cfg/etcd.conf (需要对应修改节点名称和IP)

安装验证:
/k8s/etcd/bin/etcdctl --ca-file=/k8s/etcd/ssl/ca.pem --cert-file=/k8s/etcd/ssl/server.pem --key-file=/k8s/etcd/ssl/server-key.pem --endpoints="https://172.28.80.207:2379,https://172.28.80.205:2379,https://172.28.80.211:2379" cluster-health

member aa5b8ff59052d9e5 is healthy: got healthy result from https://172.28.80.211:2379
member aa9960aee2a3c3df is healthy: got healthy result from https://172.28.80.205:2379
member f8f89d259b5d43dd is healthy: got healthy result from https://172.28.80.207:2379

```

#### 安装网络

```
cd /k8s/etcd/ssl/
/k8s/etcd/bin/etcdctl \
--ca-file=ca.pem --cert-file=server.pem \
--key-file=server-key.pem \
--endpoints="https://172.28.80.207:2379,\
https://172.28.80.205:2379,https://172.28.80.211:2379" \
set /coreos.com/network/config  '{ "Network": "10.18.0.0/16", "Backend": {"Type": "vxlan"}}'

FLANNEL_OPTIONS="--etcd-endpoints=https://172.28.80.207:2379,https://172.28.80.205:2379,https://172.28.80.211:2379 -etcd-cafile=/k8s/etcd/ssl/ca.pem -etcd-certfile=/k8s/etcd/ssl/server.pem -etcd-keyfile=/k8s/etcd/ssl/server-key.pem"

```

#### 部署

Kube-apiserver
```
/k8s/kubernetes/cfg/kube-apiserver
/usr/lib/systemd/system/kube-apiserver.service
````
kube-scheduler
```
/k8s/kubernetes/cfg/kube-scheduler
/usr/lib/systemd/system/kube-apiserver.service
```
kube-controller-manager
```
/k8s/kubernetes/cfg/kube-controller-manager
/usr/lib/systemd/system/kube-controller-manager.serivce
```


apiserver 访问 kubelet 需要权限，配置下列参数
```
# kubectl exec   error: unable to upgrade connection: Forbidden (user=system:anonymous, verb=create, resource=nodes, subresource=proxy)
# apiserver --kubelet-client-certificate  --kubelet-client-key
# kubelet    --client-ca-file

kubectl create clusterrolebinding the-boss --user  kubernetes --clusterrole cluster-admin
```



node
```
BOOTSTRAP_TOKEN  a9ae897d0c1e727ded826325f7d7bb0f
用户 kubelet-bootstrap
用户组 system:bootstrapper
角色 system:node-bootstrapper　
```
> 用户，用户组，bootstrapping 填坑
> https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet-tls-bootstrapping/


```
# 创建kubelet bootstrapping kubeconfig
BOOTSTRAP_TOKEN=a9ae897d0c1e727ded826325f7d7bb0f
KUBE_APISERVER="https://172.28.80.207:6443"
# 设置集群参数
kubectl config set-cluster kubernetes \
  --certificate-authority=/k8s/kubernetes/ssl/ca.pem \
  --embed-certs=true \
  --server=${KUBE_APISERVER} \
  --kubeconfig=bootstrap.kubeconfig

# 设置客户端认证参数
kubectl config set-credentials kubelet-bootstrap \
  --token=${BOOTSTRAP_TOKEN} \
  --kubeconfig=bootstrap.kubeconfig

# 设置上下文参数
kubectl config set-context default \
  --cluster=kubernetes \
  --user=kubelet-bootstrap \
  --kubeconfig=bootstrap.kubeconfig

# 设置默认上下文
kubectl config use-context default --kubeconfig=bootstrap.kubeconfig

#----------------------

# 创建kube-proxy kubeconfig文件

kubectl config set-cluster kubernetes \
  --certificate-authority=/k8s/kubernetes/ssl/ca.pem \
  --embed-certs=true \
  --server=${KUBE_APISERVER} \
  --kubeconfig=kube-proxy.kubeconfig

kubectl config set-credentials kube-proxy \
  --client-certificate=/k8s/kubernetes/ssl/kube-proxy.pem \
  --client-key=/k8s/kubernetes/ssl/kube-proxy-key.pem \
  --embed-certs=true \
  --kubeconfig=kube-proxy.kubeconfig

kubectl config set-context default \
  --cluster=kubernetes \
  --user=kube-proxy \
  --kubeconfig=kube-proxy.kubeconfig

kubectl config use-context default --kubeconfig=kube-proxy.kubeconfig

```


 kubelet 参数配置模板文
 > /k8s/kubernetes/cfg/kubelet.config

 kubelet 配置文件
> /k8s/kubernetes/cfg/kubelet



## 部署 kube-proxy 组件
> /k8s/kubernetes/cfg/kube-proxy

## 创建管理员
> https://blog.csdn.net/xiaomin1991222/article/details/84879610


```
# 配置一个名为default的集群，并指定服务地址与根证书

KUBE_APISERVER="https://10.0.2.4:6443"
kubectl config set-cluster default \
  --certificate-authority=/k8s/kubernetes/ssl/ca.pem \
  --embed-certs=true \
  --server=${KUBE_APISERVER} \
  --kubeconfig=admin.kubeconfig



# 设置一个管理用户为admin，并配置访问证书

kubectl config set-credentials admin \
  --client-certificate=/k8s/kubernetes/ssl/server.pem \
  --client-key=/k8s/kubernetes/ssl/server-key.pem \
  --embed-certs=true \
  --kubeconfig=admin.kubeconfig

# 设置一个名为default使用default集群与admin用户的上下文，

kubectl config set-context default \
  --cluster=default \
  --user=admin \
  --kubeconfig=admin.kubeconfig

# 启用default为默认上下文
kubectl config use-context default --kubeconfig=admin.kubeconfig
```

# 创建配置文件

```

cp admin.kubeconfig ~/.kube/config
/etc/profile.d/kube.sh  把kubectl 加到path

```



## 同意
```
kubectl get csr
 kubectl certificate approve node-csr-ddB3GwD94NKP1ZJ8Rfn_cZe5eOWL_MkX9VLdEI5pAmY

```



# 部署 DNS

https://github.com/coredns/deployment/tree/master/kubernetes github 部署说明
https://blog.csdn.net/wyc_cs/article/details/88177007  安装CoreDNS实现Kubernetes的服务发现
https://blog.csdn.net/ccy19910925/article/details/80762025  kubernetes上的服务发现-CoreDNS配置

```




```

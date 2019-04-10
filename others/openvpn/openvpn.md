
#### 服务器安装

http://blog.csdn.net/defeattroy/article/details/42523175
https://www.cnblogs.com/fuyanwen/p/8195956.html


#### 客户端安装

官网下载直接安装 openvpn-install-2.3.12-I601-x86_64.exe






#### 配置文件中对应的字段文件对应关系

按照官方文档生成证书和密钥匙

- ca 文件，用来签名 server 和 client 公钥密钥
- ca 签名正确的情况下， server client 分别通过 各自的 公钥密钥 互相验证

> sample/openvpn

    ca    ca.crt
    cert  client.crt
    key   client.key



#### 运行

     openvpn --config C:\Users\test\Documents\04_soft\UnoTelly_US_OpenVPN.ovpn


#### 服务器配置关键


dev tun         工作在三层网络
topology subnet 网络方式，子网

push "route 192.168.122.0 255.255.255.0"   添加 服务器侧子网路由
push "route 192.168.150.0 255.255.255.0"   添加 服务器侧子网路由

push "route 115.236.59.94 255.255.255.255 net_gateway"   所有的路由走VPN，除了ssh
push "redirect-gateway def1"                            所有的路由走VPN

#### 通过 ssh 运行

     服务器 upd 改tcp

     $ ssh -l root -N -L1194:192.168.122.85:1194  115.236.59.94            
     \UnoTelly_US_OpenVPN.ovpn server 改为 127.0.0.1

#### 配置客户端固定 IP
ccd 目录中创建 client1 （客户端证书  Common Name ）
ifconfig-push 10.8.0.11 255.255.255.0


#### client 路由

实际操作无效
route.exe ADD 192.168.122.0 MASK 255.255.255.0 10.8.0.1

在server 端配置 push 192.168.122.0 255.255.0





#### 坑坑



comp-lzo 要和服务器一致，否则会无法连接

     comp-lzo  

指定认证类型

    ns-cert-type server

路由使用 server push 生效，客户端配置无效，

    tracert 工具初步定位为 windows问题，但是也不排除是其他问题，挂起




### window 客户端

- 安装openvdn
- 使用管理员启动 cmd.exe
-  openvpn --config C:\Users\test\Documents\04_soft\94-s5.ovpn

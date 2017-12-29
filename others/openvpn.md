
#### 服务器安装

http://blog.csdn.net/defeattroy/article/details/42523175 CentOS6.5 64位安装openvpn

#### 客户端安装

官网下载直接安装 openvpn-install-2.3.12-I601-x86_64.exe





#### 配置文件中对应的字段文件对应关系

> sample/openvpn

    ca    ca.crt
    cert  client.crt
    key   client.key






#### 运行

     openvpn --config C:\Users\test\Documents\04_soft\UnoTelly_US_OpenVPN.ovpn


#### 通过 ssh 运行

     服务器 upd 改tcp
     $ ssh -l root -N -L1194:192.168.122.85:1194  115.236.59.94            
     \UnoTelly_US_OpenVPN.ovpn server 改为 127.0.0.1




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

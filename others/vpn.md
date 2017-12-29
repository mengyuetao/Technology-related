http://www.linuxfly.org/post/641/
http://jpuyy.com/2011/09/pptpsetup-dial-pptp-vpn.html







## Linux Install

安装工具

	view/
	yum install pptp
	yum install pptp-setup

VPN配置 

	pptpsetup --create vpn_company  --server vpn.company.com --user vpn_user 
	--password vpn_pass --encrypt

检查配置结果

	cat /etc/ppp/chap-secrets 
	vpn_user vpn_company "vpm_pass" *
	cat /etc/ppp/peers/vpn_company
	pty "pptp vpn.company.com --nolaunchpppd"
	lock
	refuse-eap
	noauth
	nobsdcomp
	nodeflate
	name vpn_user
	remotename vpn_company
	ipparam vpn_company
 	require-mppe-128

注册内核模块
	
modprobe ppp_mppe

连接VPN
记得关闭防火墙
	pppd call vpn_company

检查连接状态
 检查日志 
	
	/var/log/messages

断开连接
	
	killall pppd

   


## Linux form outside into inside

目的：金牌厨房版本发布，通过一种简单的方法，把实验环境链接到外网地址，方便测试。
时间：20150510
作者：孟越涛
结果：经过本人测试通过。
总体思路
1 外网服务器启动 VNP
2 内网链接 VNP ，与外网加入同一个子网
3 通过子网路由到内网服务，（需在外网添加路由）
4 外网服务 转到内网 （外网服务器先 DNAT ， 再 SNAT）
具体步骤：
1 外网服务器搭建 pptp 参考
http://www.jb51.net/os/RedHat/128137.html
http://www.2cto.com/os/201306/218774.html

2 内网服务器搭建 ppp client
http://www.linuxidc.com/Linux/2014-07/104734.htm
http://blog.secaserver.com/2012/12/centos-6-install-vpn-pptp-client-simple/
3 在VNP服务器添加路由 route add -net 192.168.1.0 netmask 255.255.255.0 dev ppp0
4 添加包转发
iptables -t nat -A PREROUTING -d 115.28.17.83 -p tcp --dport 8080 -j DNAT --to 192.168.1.102:80
iptables -t nat -A POSTROUTING -o ppp0 -j MASQUERADE
测试：
内网搭建nginx ， 在VPN服务器 curl 主页，在外网 curl主页。
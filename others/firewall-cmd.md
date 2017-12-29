
## 防火墙配置

加载系统默认配置，默认只打开22端口
1 firewall-cmd --reloadd 加载默认配置
 
发现kvm有一个Bug，添加第四条规则导致，导致外网Forward失败，删除该规则。
2 iptables -D FORWARD 4


3 添加外网到内网的映射

重启后失效:
firewall-cmd --zone=public --add-forward-port=port=80:proto=tcp:toport=8080:toaddr=192.168.122.111

永久:
firewall-cmd --permanent  --add-forward-port=port=62021:proto=tcp:toport=8800:toaddr=192.168.122.21
一个小时后失效:
firewall-cmd  --add-forward-port=port=55021:proto=tcp:toport=3389:toaddr=192.168.122.21  --timeout=3600
4添加开放端口（虚拟机执行）
(centos7.0) firewall-cmd --add-port=8080/tcp
(centos6.5) iptables -F    （非安全）    

5 由于80端口内网影射导致，无法访问外网解决:
kvm给122网段添加例外-访问出去：iptables -t mangle -A PRE_public_allow -s 192.168.122.0/24 -j MARK --set-mark 0x0               
kvm允许访问内网段-返回数据：firewall-cmd --add-masquerade  
参考文档：http://fedoraproject.org/wiki/FirewallD
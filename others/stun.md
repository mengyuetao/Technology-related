
# 安装
https://www.dazhuanlan.com/2020/03/15/5e6ddc5446b43/   Ubuntu安装coturn服务
https://blog.csdn.net/righteousness/article/details/90732368 编译安装coturn小记
http://www.srcmini.com/61713.html 如何在Ubuntu 18.04中使用Coturn创建和配置自己的STUN/TURN服务器
https://blog.csdn.net/qq_38607742/article/details/111242992 Centos配置turn服务器
https://blog.csdn.net/polarGW/article/details/70226634 CenterOS7.2&Ubuntu14.04配置TURN服务器

https://github.com/coturn/coturn

https://www.gdbgui.com/installation/ gdb调试工具


```
方法1
yum install coturn
yum install coturn-utils

方法2
wget https://github.com/coturn/coturn/archive/4.5.2.tar.gz
tar -xvf 4.5.2.tar.gz
yum install openssl-devel
yum install libevent-devel
cd coturn-4.5.2/
./configure
make install

firewall-cmd --zone=public --add-port=49152-65535/udp
firewall-cmd --zone=public --add-port=3478/udp

https://webrtc.github.io/samples/src/content/peerconnection/trickle-ice/ 测试

cat /var/log/coturn/turnserver.log


代码阅读
stun_is_challenge_response_str


```

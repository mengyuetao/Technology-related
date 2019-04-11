

## 环境

sshd_config GatewayPorts yess

route add default gw 192.168.56.1
chmod  600 id_rsa
autossh -M 50101 -f  -l root -N  -R 0.0.0.0:60101:127.0.0.1:22 47.105.203.62

route add default gw 192.168.56.1
chmod  600 id_rsa
autossh -M 50102 -f  -l root -N  -R 0.0.0.0:60102:127.0.0.1:22 47.105.203.62

route add default gw 192.168.56.1
chmod  600 id_rsa
autossh -M 50103 -f  -l root -N  -R 0.0.0.0:60103:127.0.0.1:22 47.105.203.62

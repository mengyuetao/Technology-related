

### 最小化安装
CentOS-6.5-x86_64-bin-DVD1to2

### 升级 Linux 源

rpm -Uvh http://mirror.webtatic.com/yum/el6/latest.rpm


###安装php
yum install phpw54


###安装mysql 5.7
1. 参考官网
2. 允许远程访问    
3.
> mysql>update mysql.user set host = '%' where user = 'root';
>
> mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'MyNewPass4!' WITH GRANT OPTION;



###zabbix源码3.0

0.  yum -y groupinstall "Development Tools" 安装编译工具

1. 安装参考官网 源码安装章节，安装必要的依赖。
2. 修改路径权限
   > chown apache.apache -R /var/www/html
   > chcon -R -u system_u -t httpd_sys_content_t /var/www/html/zabbix


### seLinux 设置
1. setsebool httpd_can_network_connect on  立即生效
2. setsebool -P httpd_can_network_connect on 重启后生效

>参考 http://blog.csdn.net/windowsxpwyd/article/details/7590202

>SELinux为Enforcing模式时安装Zabbix1.8.13

### 时钟矫正

ntpdate -u time.windows.com


### agent
 /etc/zabbix/agentd.conf 配置服务器IP , 否则数据没法获取




#源码

groupadd zabbix
 useradd -g zabbix zabbix

 tar -zxvf zabbix-3.0.3.tar.gz
cd zabbix-3.0.3
 ./configure --enable-agent
make install
vi /usr/local/etc/zabbix_agentd.conf
zabbix_agentd

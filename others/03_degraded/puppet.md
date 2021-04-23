
leo:2016-9-8 first edition


## Install

puppet verion:4.6.2 on centos 6.5

#### server
    sudo rpm -Uvh https://yum.puppetlabs.com/puppetlabs-release-pc1-el-6.noarch.rpm
    yum install puppetserver
    service puppetserver status
    service puppetserver start

#### agent
    sudo rpm -Uvh https://yum.puppetlabs.com/puppetlabs-release-pc1-el-6.noarch.rpm
    yum install puppet-agent

   /etc/profile.d/ 配置path变量


config agent's master

    vi /etc/puppetlabs/puppet/puppet.conf
    
    [main]
    server=manage1

    

####  run command
    
    export PATH=/opt/puppetlabs/bin:$PATH
    puppet master --configprint manifest 配置文件路径
    puppet master --configprint all      全部配置

###### on agent

    puppet agent --test


###### On Server

    puppet cert --list              Server 签名证书
    puppet cert --sign   appx1



## Caution

1. make sure agent and master have the same version
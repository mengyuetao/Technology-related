

## 安装
第一步，安装perl,gcc ,mysql, mysql-devel
  yum install perl gcc mysql mysql-devel
第二步，安装
yum install perl-ExtUtils-CBuilder perl-ExtUtils-MakeMaker 



第三步，安装 CPANm 
wget -O- http://cpanmin.us --no-check-certificate | perl - --sudo --self-upgrade

第四步，安装依赖的库
cpanm --mirror http://mirrors.163.com/cpan --mirror-only Time::HiRes
 
//同上安装下面的包
 cpan -i Time::HiRes
  cpan -i Danga::Socket 
  cpan -i  BSD::Resource
  cpan -i  LWP::UserAgent 
  cpan -i HTTP::Response
  cpan -i HTTP::Request

第三步，MogileFS 快速上手,需要连接 VPN(慢) 还是使用 --mirror
cpanm --mirror http://mirrors.163.com/cpan --mirror-only MogileFS::Server
cpanm --mirror http://mirrors.163.com/cpan –-mirror-only MogileFS::Utils
cpanm --mirror http://mirrors.163.com/cpan –-mirror-only MogileFS::Client
cpanm --mirror http://mirrors.163.com/cpan –-mirror-only DBD::mysql





## 备份MogileFs

#### install mogbak

######  [install rubby](https://github.com/postmodern/ruby-install#readme)

    wget -O ruby-install-0.6.0.tar.gz https://github.com/postmodern/ruby-install/archive/v0.6.0.tar.gz
    tar -xzvf ruby-install-0.6.0.tar.gz
    cd ruby-install-0.6.0/
    sudo make install

    ruby-install ruby


安装命令行 1.6.0 删除其他版本 2.4.0
     
    gem list gli
    gem uninstall gli -v 2.4.0
    gem install gli -v 1.6.0


编译安装 mogbak 
  
    yum install sqlite-devel
    yum install mysql-devel    

    git clone https://github.com/firespring/mogbak.git
    gem build mogbak.gemspec
    gem install
    gem install mogbak-0.2.2.gem 


#### 备份

     ./mogbak create --db=MogileFS --dbhost=192.168.2.10 --dbpass=jpcf --dbuser=jpcf --domain=jpcf --trackerip=192.168.2.10  /backups/jpcf

     ./mogbak backup /backups/jpcf

     ./mogbak restore  --domain=jpcf --trackerip=192.168.2.10  /backups/jpcf


[cmd-syntax](https://github.com/firespring/mogbak/wiki/Command-syntax)

      

     ./mogbak create --db=MogileFS --dbhost=127.0.0.1 --dbpass=jpcf_yeg_2015  --dbuser=root --domain=ycdomain   /backups/jpcf


 
注意： MogileFS 数据库还原后，目录关系会错。 需要修改相应的设备目录，主机啥的。


## 服务启动

    su mogile
    mogilefsd -c /usr/common/mogilefs/mogilefsd.conf --daemon

    mogstored -c /usr/common/mogilefs/storage.conf --daemon


## 显示状态
mogstats --db_dsn="DBI:mysql:MogileFS:host=192.168.2.10" --db_user="jpcf" --db_pass="jpcf" --verbose --stats="all"
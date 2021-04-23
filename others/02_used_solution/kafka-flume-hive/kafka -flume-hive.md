# 概述
https://www.cnblogs.com/smartloli/p/9984140.html 使用Flume消费Kafka数据到HDFS
https://blog.csdn.net/buster2014/article/details/50716242 【源码分析】flume中sink到hdfs，文件系统频繁产生文件，文件滚动配置不起作用？

数据收集
kafka -> flume -> hdfs -> hive

# 步骤说明
- 安装 hadoop
- 安装 flume
- 安装 hive

# 安装 hadoop

```
#安装：

/etc/profile.d/javahome.sh   # JAVA_HOME 配置环境变量
source /etc/profile.d/javahome.sh # 变量生效

/opt/hadoop/hadoop-2.9.2     # 部署（解压2.9.2 jar解压到此路径）

#配置：
/opt/hadoop/hadoop-2.9.2/etc/hadoop/core-site.xml  
/opt/hadoop/hadoop-2.9.2/etc/hadoop/hdfs-site.xml  
/opt/hadoop/hadoop-2.9.2/etc/hadoop/mapred-site.xml
/opt/hadoop/hadoop-2.9.2/etc/hadoop/yarn-site.xml

#服务：
bin/hdfs namenode -format  # 第一次
sbin/start-dfs.sh
sbin/start-yarn.sh

```

# 安装 flume

```
#编译插件(可选，已经提供)
pushd /home/mengyuetao/Desktop/flume-src/flume-ng-sinks/flume-hdfs-sink
../../mvnw dependency:copy-dependencies -DoutputDirectory=./libdep2
popd


#安装：
/opt/apache-flume-1.10.0-SNAPSHOT-bin #解压，我直接取的是源码版本编译，可以直接下载 1.9.0版本解压
/opt/apache-flume-1.10.0-SNAPSHOT-bin/plugins.d/hdfs-sink   #安装插件 hdfs-sink 的以来jar（很关键），这个网上没有，需要源码编译

#配置：
/opt/apache-flume-1.10.0-SNAPSHOT-bin/conf/kafka2hdfs.properties  #当前配置从 example 的topic 同步数据，更具实际情况修改

#启动：
./bin/flume-ng agent -n agent   --conf conf --conf-file  ./conf/kafka2hdfs.properties

```

# 安装 hive

```
环境配置
/etc/profile.d/hive.sh   #  配置环境变量
source /etc/profile.d/hive.sh #变量生效

#安装：
/opt/hive/apache-hive-2.3.5-bin
#配置
/opt/hive/apache-hive-2.3.5-bin/hive-site.xml

#启动：
nohup hive --service hiveserver2 1>> /tmp/hs2.log 2>> /tmp/hs2.log &
nohup hive --service metastore 1>> /tmp/meta.log 2>> /tmp/meta.log &

```

json
https://web.archive.org/web/20190217104719/https://blogs.msdn.microsoft.com/bigdatasupport/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight/

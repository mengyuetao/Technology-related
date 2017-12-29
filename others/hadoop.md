


## 命令


hdfs namenode：

    /root/hadoop-2.7.2/sbin/hadoop-daemons.sh --config /root/hadoop-2.7.2/etc/hadoop --hostnames app1 --script /root/hadoop-2.7.2/sbin/hdfs start namenode

hdfs datanode :

    /root/hadoop-2.7.2/sbin/hadoop-daemons.sh --config /root/hadoop-2.7.2/etc/hadoop --script /root/hadoop-2.7.2/sbin/hdfs start datanode


yarn resourcemanager：
    /root/hadoop-2.7.2/sbin/yarn-daemon.sh --config /root/hadoop-2.7.2/etc/hadoop start  resourcemanager

yarn nodemanager
    /root/hadoop-2.7.2/sbin/yarn-daemons.sh --config /root/hadoop-2.7.2/etc/hadoop start  nodemanager
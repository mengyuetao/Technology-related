


http://www.xmsxmx.com/kafka-consumer-offset-management/



## kafka 总结

###存储结构
1. 采用二进制，消息顺序存储。 通过多个文件分段提高性能。
2. 单个文件检索采用稀释索引提高性能。

###可靠性
1. 一个leader, 多个follower。 leader挂掉在catchup的follower里选择leader。
2. 0不考虑是否commit, 1 leader commit , -1 所有的都commit. 0，1，-1 选项在可用性可可靠性上权衡。



kafka

http://www.confluent.io/blog/author/jay-kreps/

http://www.confluent.io/blog/stream-data-platform-2/





## 命令

cd kafka_2.11-0.10.0.0

  164  bin/kafka-server-start.sh config/server.properties
  165  nohup bin/kafka-server-start.sh config/server.properties &



## compact 相关参数

compact  只会compact 分激活的segment.所以设置 segment.ms ，让sagment 滚动， 关于key非null，值null的数据（手册上说会删除，实际还没有看到，单是null值会更引起前面的数据被delete）

min.cleanable.dirty.ratio=0.1,  
delete.retention.ms=10000,            这个时间觉得删除标志的数据实际删除延时
segment.ms=300000,                    生成新的segment的时间
cleanup.policy=compact

  bin/kafka-topics --zookeeper 192.168.2.22:2181,192.168.2.23:2181,192.168.2.24:2181 --topic supplyEndpointOrder --describe
  Topic:supplyEndpointOrder	PartitionCount:1	ReplicationFactor:1	Configs:min.cleanable.dirty.ratio=0.1,delete.retention.ms=10000,segment.ms=300000,cleanup.policy=compact
  	Topic: supplyEndpointOrder	Partition: 0	Leader: 1	Replicas: 1	Isr: 1


    #### kafka

    ```
    ./kafka-topics --zookeeper 192.168.2.22:2181,192.168.2.23:2181,192.168.2.24:2181 --topic  supplyEndpointOrder  --config min.cleanable.dirty.ratio=0.1 delete.retention.ms=10000 segment.ms=300000,cleanup.policy=compact  --alter
    ./kafka-topics --zookeeper 192.168.2.22:2181,192.168.2.23:2181,192.168.2.24:2181 --topic _schemas --describe
    ./kafka-console-consumer --zookeeper 192.168.2.22:2181,192.168.2.23:2181,192.168.2.24:2181  --topic _schemas --from-beginning  --property  print.key=true
    ./kafka-console-producer --broker-list  app3:9092,app4:9092,app5:9092 --topic _schemas --property parse.key=true
    bin/kafka-simple-consumer-shell --partition 0  --print-offsets  --broker-list 192.168.2.22:9092,192.168.2.23:9092,192.168.2.24:9092  --topic supplyEndpointOrder  --offset -2  --property  print.key=true

    bin/kafka-topics --zookeeper 192.168.2.22:2181,192.168.2.23:2181,192.168.2.24:2181  --create --topic supplyEndpointOrder05 --partitions 1  --config min.cleanable.dirty.ratio=0.1 --config delete.retention.ms=10000 --config segment.ms=300000 --config cleanup.policy=compact  --replication-factor 3

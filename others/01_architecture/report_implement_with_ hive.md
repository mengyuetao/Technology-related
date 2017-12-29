
## overview

通过架构改进，探索相对可靠，可扩展，简单的定时统计分析架构


参考
https://www.confluent.io/blog/how-to-build-a-scalable-etl-pipeline-with-kafka-connect/

###  steps
1. 启动 confluent schema Register
2. 启动 samzajob: MaxwellAvroConvertDispatchTask
3. 启动 conflulent connect 或 gobblin
4. 使用 hive 计算


#### gobblin ETL

优点：组件化，稳定可靠，功能多，定制性超强，支持各种适配（JDBC，HDFS，KAFKA,REST）
缺点：使用复杂，入门高，不支持实时，HDFS publish kafka 不完善

gobblin standlone , 注意点，如有失败删除上次失败 job的locks文件；

```
dfs -rm -r -f /root/gobblin/work/locks
export GOBBLIN_JOB_CONFIG_DIR=/root/gobblin/jobconfig
export GOBBLIN_WORK_DIR=/root/gobblin/work
export HADOOP_HOME=/opt/hadoop/hadoop-2.7.3
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.101-3.b13.el6_8.x86_64

./bin/gobblin-standalone.sh start
./bin/gobblin-standalone.sh stop


job.name=supplyEndpointOrder
job.group=hhkj
job.description=get topic EndpointOrder from maxwell
job.lock.enabled=true


配置文件 supplyEndpointOrder.pull

source.class=gobblin.source.extractor.extract.kafka.KafkaDeserializerSource
kafka.deserializer.type=CONFLUENT_AVRO
kafka.schema.registry.url=http://192.168.2.20:8081
kafka.brokers=192.168.2.22:9092,192.168.2.23:9092,192.168.2.24:9092
topic.whitelist=supplyEndpointOrder-0-2
bootstrap.with.offset=earliest


fs.uri=hdfs://192.168.2.20:9000
writer.fs.uri=hdfs://192.168.2.20:9000
state.store.fs.uri=hdfs://192.168.2.20:9000

extract.namespace=gobblin.extract.kafka



writer.builder.class=gobblin.writer.AvroDataWriterBuilder
writer.file.path.type=tablename
writer.destination.type=HDFS
writer.output.format=AVRO

data.publisher.type=gobblin.publisher.BaseDataPublisher

metrics.reporting.file.enabled=true
metrics.reporting.file.suffix=txt


task.data.root.dir=/gobblin-kafka
state.store.dir=/gobblin-kafka/state-store
data.publisher.final.dir=/gobblin-kafka/job-output
mr.job.root.dir=/gobblin-kafka/working
mr.job.max.mappers=3


```



####  confluent kafka  connect

优点：使用简单，专门基于kafka场景，实时性强。
缺点：不够成熟，可定制性低，不稳定，还在发展中。

``
bin/connect-distributed etc/schema-registry/connect-avro-distributed.properties
```


配置样例

```
hive hdfs connet

name=hdfs-sink
connector.class=io.confluent.connect.hdfs.HdfsSinkConnector
partitioner.class=io.confluent.connect.hdfs.partitioner.HourlyPartitioner
tasks.max=1
topics=supplyEndpointOrder_0_2
hdfs.url=hdfs://localhost:9000
flush.size=3
hive.integration=true
hive.metastore.uris=thrift://192.168.2.20:8083 # FQDN for the host part
hive.database=hhkj
schema.compatibility=BACKWARD
locale=zh
timezone=Asia/Shanghai


curl -X POST -H "Content-Type: application/json" --data '{"name": "hdfs-sink",
 "config": {
   "connector.class":"io.confluent.connect.hdfs.HdfsSinkConnector",
   "partitioner.class":"io.confluent.connect.hdfs.partitioner.HourlyPartitioner",
  "tasks.max":"1",  
   "topics":"supplyEndpointOrder-0-2",
   "hdfs.url":"hdfs://192.168.2.20:9000",
   "flush.size":"3",
   "hive.integration":"true",
   "hive.metastore.uris":"thrift://192.168.2.20:8083",
   "hive.database":"hhkj",
   "schema.compatibility":"BACKWARD",
   "path.format":"'year'=YYYY/'month'=MM/'day'=dd/'hour'=HH/",
   "locale":"zh",
   "timezone":"Asia/Shanghai"
  }
}' http://192.168.2.20:8083/connectors



```


#### hive 计算(例子)

1. 对数据分区处理

```
create table hello( endpointOrderid string  ,endpointId string , totalmoney string , createtime string ,updatetime string) partitioned by (ds string);
INSERT OVERWRITE TABLE hello PARTITION (ds)
select endpointOrderid ,endpointId, totalmoney, regexp_extract(createtime,".*-.*-.* ",0) as ds, createtime, updatetime  from supplyendpointOrder;

```

1. 数据去重复、聚合分组统计

```
create table s_sales_day( num  string  , totalmoney string , day string ) ;

按天计算销量和数量
INSERT OVERWRITE TABLE s_sales_day
select count(endpointorderid), sum(totalmoney) , ds  from (select endpointorderid ,totalmoney , updatetime ,regexp_extract(updatetime,".*-.*-[0-9]*",0) as ds ,  row_number() over (partition by endpointorderid order by updatetime desc ) num from supplyEndpointOrder_0_2 where updatetime> "2017-03-01 00:00:00" )  t where t.num =1 group by ds

```

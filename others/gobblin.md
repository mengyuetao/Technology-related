
##编译

编译版本 0.8.0 +


1. 必须使用 jdk , 1.8 可以使用，1.9 不行
2. 采用 gradew ，自动下载版本 2.13 ,其他版本不行
3. eclipse 需要安装  lombok
4. eclipse window 当前状态，使用 ecliplse task，导入，可以编译。不能使用(window路径兼容问题)
5. eclipse ubunt 直接使用 grade导入，eclispe 插件会有库冲突。
6. eclipse complie 需要把 隐藏 api 使用 error 改为 warning,否则无法编译
7. 编译 gradlew assemble

## overview

gobblin read data from kafka which serialize using avro and schema register (get schema with id in raw message) ,
gobblin write data into avro format, which will be load using hive.


## eclipse 运行

  Main class:
  gobblin.scheduler.SchedulerDaemon

  *Programe Arguments
  ```
  /home/leo/Documents/gobblin-dist/conf/gobblin-standalone.properties
  ```

  *VM arguments
  ```
  -Xmx2g -Xms1g -XX:+UseConcMarkSweepGC -XX:+UseParNewGC -XX:+PrintGCDetails -XX:+PrintGCDateStamps -XX:+PrintTenuringDistribution -XX:+UseCompressedOops -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/home/leo/Documents/gobblin-dist/logs/ -Xloggc:/home/leo/Documents/gobblin-dist/logs/gobblin-gc.log -Dgobblin.logs.dir=/home/leo/Documents/gobblin-dist/logs -Dlog4j.configuration=file:/home/leo/Documents/gobblin-dist/conf/log4j.properties  -Dorg.quartz.properties=/home/leo/Documents/gobblin-dist/conf/quartz.properties
```

*Enviroments
```
GOBBLIN_JOB_CONFIG_DIR=/home/leo/Documents/my-test-job/job
GOBBLIN_WORK_DIR=/home/leo/Documents/my-test-job/work
HADOOP_HOME=/home/leo/Documents/hadoop-2.7.2 (2.7.3?)


```
* job config
> /home/leo/Documents/my-test-job/job/wikipedia-kafka.pull

```
job.name=GobblinKafkaQuickStart
job.group=GobblinKafka
job.description=Gobblin quick start job for Kafka
job.lock.enabled=false

kafka.brokers=192.168.2.22:9092,192.168.2.23:9092,192.168.2.24:9092

topic.whitelist=supplyEndpointOrder

source.class=gobblin.source.extractor.extract.kafka.KafkaDeserializerSource
kafka.deserializer.type=CONFLUENT_AVRO
kafka.schema.registry.url=http://192.168.2.20:8081

extract.namespace=gobblin.extract.kafka

writer.builder.class=gobblin.writer.AvroDataWriterBuilder
writer.file.path.type=tablename
writer.destination.type=HDFS
writer.output.format=AVRO

data.publisher.type=gobblin.publisher.BaseDataPublisher

mr.job.max.mappers=1

metrics.reporting.file.enabled=true
metrics.log.dir=${env:GOBBLIN_WORK_DIR}/metrics
metrics.reporting.file.suffix=txt

bootstrap.with.offset=earliest

fs.uri=hdfs://192.168.2.20:9000
writer.fs.uri=hdfs://192.168.2.20:9000
state.store.fs.uri=hdfs://192.168.2.20:9000

mr.job.root.dir=/gobblin-kafka/working
state.store.dir=/gobblin-kafka/state-store
task.data.root.dir=/jobs/kafkaetl/gobblin/gobblin-kafka/task-data
data.publisher.final.dir=/gobblintest/job-output
```


## wikipada 源码分析

- 读取 topic
- 每一个topic 一个workunit，
- 每一个workunit 读取上一次的pre_highwatermark, 里面有最后一次处理的 revision , 如果没有， 读取距离现在2天的revision
- extractor 读取 5 条数据，其实是6天，包含最后一条。
  - 如果多余5条，还是读取5条，每次读取记录workunit 的 highwatermark，结束
  - 如果少于5跳，同上

- 下一次启动返回第一步。




## hdfs sample
https://github.com/linkedin/gobblin/blob/master/gobblin-example/src/main/resources/avro-to-mysql.pull


## article
https://engineering.linkedin.com/big-data/bridging-batch-and-streaming-data-ingestion-gobblin

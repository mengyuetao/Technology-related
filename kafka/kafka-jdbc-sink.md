## 概述

- Author: Yuetao Meng
- Mail: mfty1980@sina.com
- Date: 2018-9-28 v0.1

基于kafka的数据订购和分发
本文描述　kafka, jdbc, schema-registory 相关配置

假设　
数据源　topic=YYYYNL-t_user_nlp_log-18092501 格式 [schema-registory-avro](https://docs.confluent.io/current/schema-registry/docs/serializer-formatter.html)

数据库　mysql jdbc 表test


## 安装配置
#### 安装zookeeper

> install zookeeper 3.4.12

可以共用现有的zookeeper

#### 安装 kafka

 - 下载解压 kafka 2.11_2.2.0
 - 配置文件配置
 - 启动kafka


```
# 配置文件conf/server.properties
# 单机板可以大多使用默认配置
log.dirs=/var/kafka/lib/logs/    默认为tmp 需要修改,否则会造成数据丢失
zookeeper.connect=115.236.59.94:2181 配置 zookeeper
```

```
启动
bin/kafka-server-start.sh  config/server.properties
```


#### 安装 confluent-5.0.0

- 解压 confluent-5.0.0
- 配置 schema-registery
- 启动 schema-registery
- 配置 kafka-connector   
- 启动 kafka-connector
- 加载 jdbc-connector-sink 插件到 kafka-connector

> 注意　window 解压的话一些链接文件不支持，提示错误，揭开后需要把这批链接文件替换为实际链接的文件副本

```
# 配置  schema-registery
# 文件路径/etc/schema-registry/schema-registry.properties
kafkastore.connection.url=localhost:2181  # 指定为 zookeeper地址
启动 schema-registery
文件路径  bin/schema-registry-start ./etc/schema-registry/schema-registry.properties
ctrl+z   #切换到后台
bg 1     #后台运行
disown -h %1 #与终端分离，避免关闭终端退出
```


```
重要，首先拷贝mysql jdbc driver 到　/share/java/kafka-connect-jdbc

配置 kafka-connector
文件路径 /etc/schema-registry/connect-avro-distributed.properties
bootstrap.servers=localhost:9092  #kafka 地址
key.converter.schema.registry.url=http://localhost:8081  # schema-registry 地址
value.converter.schema.registry.url=http://localhost:8081 # schema-registry 地址



启动kafka-connector
bin/connect-distributed ./etc/schema-registry/connect-avro-distributed.properties
```

　


```



＃看所有的　connectors  
curl  http://127.0.0.1:8083/connectors  

＃看名称为　YYYYNL-t_user_nlp_log-18092501-jdbc-sink的connector　
curl  http://127.0.0.1:8083/connectors/YYYYNL-t_user_nlp_log-18092501-jdbc-sink
curl  http://127.0.0.1:8083/connectors/YYYYNL-t_user_nlp_log-18092501-jdbc-sink/tasks
curl  http://127.0.0.1:8083/connectors/YYYYNL-t_user_nlp_log-18092501-jdbc-sink/tasks/0/status　　＃id 为０的task

#删除名称为　YYYYNL-t_user_nlp_log-18092501-jdbc-sink的connector
curl  -X DELETE http://127.0.0.1:8083/connectors/YYYYNL-t_user_nlp_log-18092501-jdbc-sink

＃创建名称为　YYYYNL-t_user_nlp_log-18092501-jdbc-sink的connector
curl -X POST -H "Content-Type: application/json" --data '{"name": "YYYYNL-t_user_nlp_log-18092501-jdbc-sink",
 "config": {
   "connector.class":"io.confluent.connect.jdbc.JdbcSinkConnector",
   "connection.url":"jdbc:mysql://115.236.59.94:3306/test?useUnicode=true&characterEncoding=utf8&useSSL=false",
   "connection.user":"leo",
   "connection.password":"Leo963277!",
   "insert.mode":"upsert",
   "pk.mode":"record_key",
   "pk.fields":"id",
   "tasks.max":"1",
   "auto.create":true,
   "auto.evolve":true,
   "topics":"YYYYNL-t_user_nlp_log-18092501",
   "batch.size":"100"
  }
}' http://127.0.0.1:8083/connectors



```


## 附录-调测

具体参考kafka,confluent 官方文档

https://docs.confluent.io/current/schema-registry/docs/serializer-formatter.html

####　schemma-registery 管理　举例
```
curl -X POST -H "Content-Type: application/vnd.schemaregistry.v1+json" \
  --data '{"schema":"{\"type\":\"record\",\"name\":\"myrecord\",\"fields\":[{\"name\":\"f1\",\"type\":\"string\"}]}"}' \
  http://115.236.59.94:8081/subjects/t1-value/versions

curl -X POST -H "Content-Type: application/vnd.schemaregistry.v1+json" \
  --data '{"schema": "{\"type\": \"string\"}"}' \
   http://localhost:8081/subjects/Kafka-value/versions
{"id":1}

curl -X POST -H "Content-Type: application/vnd.schemaregistry.v1+json" \
  --data '{"schema": "{\"type\": \"string\"}"}' \
   http://localhost:8081/subjects/Kafka-value/versions
{"id":1}

```

#### 收发数据 工具

```

bin/kafka-avro-console-consumer --topic "YYYYNL-t_user_nlp_log-18092501-value"   --bootstrap-server 115.236.59.94:9092 --from-beginning  --property schema.registry.url=http://115.236.59.94:8081

bin/kafka-avro-console-producer --broker-list localhost:9092 --topic t2 \
  --property parse.key=true \
  --property key.schema='{"type":"string"}' \
  --property value.schema='{"type":"record","name":"myrecord","fields":[{"name":"f1","type":"string"}]}'

  bin/kafka-avro-console-producer --broker-list localhost:9092 --topic t2 \
    --property parse.key=true \
    --property key.schema='{"type":"string"}' \
    --property value.schema=`cat schemaxx`
```

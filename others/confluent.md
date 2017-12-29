
#### reference

- https://www.confluent.io/blog/unifying-stream-processing-and-interactive-queries-in-apache-kafka/
- https://www.confluent.io/blog/stream-data-platform-2/
- https://www.quora.com/Should-I-use-Gobblin-or-Spark-Streaming-to-injest-data-from-Kafka-to-HDFS
- https://www.infoq.com/presentations/etl-streams
- https://www.oreilly.com/ideas/the-world-beyond-batch-streaming-101
- https://www.oreilly.com/ideas/the-world-beyond-batch-streaming-102
- https://www.confluent.io/blog/introducing-kafka-streams-stream-processing-made-simple/



####schema-registry

启动
    cd /opt/confluent-3.0.1/
    nohup ./bin/schema-registry-start ./etc/schema-registry/schema-registry.properties &


```
注册schema,把supplyEndpointOrder.avsc转成string,

sed ':t;N;s/\n//;b t'

提交rest api ,post http://192.168.2.20:8081/subjects/supplyEndpointOrder-value/versions

POST
http://192.168.2.20:8081/subjects/supplyEndpointOrder-value/versions

Content-Type application/json


{"schema":"{  \"type\": \"record\",  \"namespace\": \"com.jtb.jpcf.schema.avro\",  \"name\": \"SupplyEndpointOrder\",  \"fields\": [{    \"name\": \"EndpointOrderId\",    \"type\": \"string\"  }, {    \"name\": \"EndpointId\",    \"type\": \"string\"  }, {    \"name\": \"TotalMoney\",    \"type\": \"string\"  }, {    \"name\": \"UserPayType\",    \"type\": {      \"name\": \"UserPayType\",      \"type\": \"enum\",      \"symbols\": [\"alipay\"]    }  }, {    \"name\": \"UserPayStatus\",    \"type\": {      \"name\": \"UserPayStatus\",      \"type\": \"enum\",      \"symbols\": [\"NotPay\", \"PaidNotComfirm\", \"PaidComfirm\", \"PayError\"]    }  }, {    \"name\": \"UserPayResultMsg\",    \"type\": \"string\"  }, {    \"name\": \"SalesmanId\",    \"type\": \"string\",    \"default\": \"\"  }, {    \"name\": \"SalesmanFranchiserId\",    \"type\": [\"string\"],    \"default\": \"\"  }, {    \"name\": \"FranchiserPayStatus\",    \"type\": {      \"name\": \"FranchiserPayStatus\",      \"type\": \"enum\",      \"symbols\": [\"NotPay\", \"PaidNotComfirm\", \"PaidComfirm\"]    }  }, {    \"name\": \"OrderSource\",    \"type\": {      \"name\": \"OrderSource\",      \"type\": \"enum\",      \"symbols\": [\"plt\"]    }  }, {    \"name\": \"CreateTime\",    \"type\": \"string\"  }, {    \"name\": \"UpdateTime\",    \"type\": \"string\"  }, {    \"name\": \"SupplyProducts\",    \"type\": {      \"type\": \"array\",      \"items\": {        \"type\": \"record\",        \"name\": \"SupplyProduct\",        \"fields\": [{          \"name\": \"ProductId\",          \"type\": \"string\"        }, {          \"name\": \"Count\",          \"type\": \"string\"        }, {          \"name\": \"Unit\",          \"type\": \"string\"        }, {          \"name\": \"Price\",          \"type\": \"string\"        }, {          \"name\": \"DealPrice\",          \"type\": \"string\"        }, {          \"name\": \"Amount\",          \"type\": \"string\"        }, {          \"name\": \"UnitRatio\",          \"type\": \"string\"        }, {          \"name\": \"BrandName\",          \"type\": \"string\"        }, {          \"name\": \"Name\",          \"type\": \"string\"        }, {          \"name\": \"ProductPic\",          \"type\": \"string\"        }, {          \"name\": \"Type\",          \"type\": \"string\"        }, {          \"name\": \"PromotionId\",          \"type\": \"string\"        }]      }    }  }]}"}



```    

#### kafka connect

启动

```
bin/connect-distributed etc/schema-registry/connect-avro-distributed.properties
```


配置样例


```

./bin/kafka-avro-console-consumer  --from-beginning    --zookeeper 192.168.2.22:2181,192.168.2.23:2181,192.168.2.24:2181 --topic Bonus2LineOrdersWithRank3404 --property print.key=true


jdbc sink connet

name=jdbc-sink
connector.class=io.confluent.connect.jdbc.JdbcSinkConnector
connection.url=jdbc:mysql://192.168.2.15:3306/jpcf-82?useUnicode=true&characterEncoding=utf8
connection.user=jpcf
connection.password=jpcf
insert.mode=upsert

pk.mode=record_key
pk.fields=mainkey
auto.create=true

tasks.max=1
topics=Bonus2LineOrdersWithRank3404
batch.size=1


curl -X POST -H "Content-Type: application/json" --data '{"name": "jdbc-sink01",
 "config": {
   "connector.class":"io.confluent.connect.jdbc.JdbcSinkConnector",
   "connection.url":"jdbc:mysql://192.168.2.15:3306/jpcf_report_82?useUnicode=true&characterEncoding=utf8",
   "connection.user":"jpcf",
   "connection.password":"jpcf",
   "insert.mode":"upsert",
   "pk.mode":"record_value",
   "pk.fields":"Day,SalesmanId,FranchiserId,EndpointId",
   "tasks.max":"1",
   "topics":"Bonus2LineOrdersWithRank3404","ManageDtManageTaskVisit3404"
   "batch.size":"1"
  }
}' http://192.168.2.20:8083/connectors

```




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
   "topics":"supplyEndpointOrder-0-2,AvroDtBonusRuleIndexSalesReach1",
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

curl -X PUT -H "Content-Type: application/json" --data '
 {
   "connector.class":"io.confluent.connect.hdfs.HdfsSinkConnector",
   "partitioner.class":"io.confluent.connect.hdfs.partitioner.HourlyPartitioner",
  "tasks.max":"1",  
   "topics":"supplyEndpointOrder05",
   "hdfs.url":"hdfs://192.168.2.20:9000",
   "flush.size":"1",
   "path.format":"'year'=YYYY/'month'=MM/'day'=dd/'hour'=HH/",
   "locale":"zh",
   "timezone":"Asia/Shanghai",
   "hive.integration":"true",
   "hive.conf.dir":"/opt/hive/apache-hive-2.1.0-bin/conf",
   "hive.home":"/opt/hive/apache-hive-2.1.0-bin",
   "hive.database":"hhkj",
   "schema.compatibility":"BACKWARD"
}' http://192.168.2.20:8083/connectors/hdfs-sink/config


```

## kafka connect jdbc


```
curl -X POST -H "Content-Type: application/json" --data '{"name": "jdbc-sink-ManageDtManageTaskVisit3404",
 "config": {
   "connector.class":"io.confluent.connect.jdbc.JdbcSinkConnector",
   "connection.url":"jdbc:mysql://192.168.2.15:3306/jpcf_report_82?useUnicode=true&characterEncoding=utf8",
   "connection.user":"jpcf",
   "connection.password":"jpcf",
   "insert.mode":"upsert",
   "pk.mode":"record_value",
   "pk.fields":"VisitDate,SalesmanId,FranchiserId,EndpointId",
   "tasks.max":"1",
   "topics":"ManageDtManageTaskVisit3404",
   "batch.size":"1"
  }
}' http://192.168.2.20:8083/connectors


curl -X POST -H "Content-Type: application/json" --data '{"name": "jdbc-sink-Bonus2LineOrdersWithRank3404",
 "config": {
   "connector.class":"io.confluent.connect.jdbc.JdbcSinkConnector",
   "connection.url":"jdbc:mysql://192.168.2.15:3306/jpcf_report_82?useUnicode=true&characterEncoding=utf8",
   "connection.user":"jpcf",
   "connection.password":"jpcf",
   "insert.mode":"upsert",
   "pk.mode":"record_value",
   "pk.fields":"Day,SalesmanId,FranchiserId,EndpointId",
   "tasks.max":"1",
   "topics":"Bonus2LineOrdersWithRank3404",
   "batch.size":"1"
  }
}' http://192.168.2.20:8083/connectors


```





#### install


1. 配置


hadoop路径配置

```
hive-env.sh
```

环境变量配置

/etc/profile.d

```
export HIVE_HOME=/opt/hive/apache-hive-2.1.0-bin
PATH=$PATH:$HIVE_HOME/bin
```


2. hive_site.xml 配置jdbc

```
<configuration>
  <property>
    <name>hive.metastore.warehouse.dir</name>
    <value>/user/hive/warehouse</value>
    <description>location of default database for the warehouse</description>
  </property>

  <property>
    <name>javax.jdo.option.ConnectionPassword</name>
    <value>jpcf</value>
    <description>password to use against metastore database</description>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionURL</name>
    <value>jdbc:mysql://192.168.2.15/hive_db?createDatabaseIfNotExist=true</value>
    <description>
      JDBC connect string for a JDBC metastore.
      To use SSL to encrypt/authenticate the connection, provide database-specific SSL flag in the connection URL.
      For example, jdbc:postgresql://myhost/db?ssl=true for postgres database.
    </description>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionDriverName</name>
    <value>com.mysql.jdbc.Driver</value>
    <description>Driver class name for a JDBC metastore</description>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionUserName</name>
    <value>jpcf</value>
    <description>Username to use against metastore database</description>
  </property>
</configuration>
```

3. 初始化 schema

bin/schematool -dbType mysql -initSchema --verbose


4. 启动 server （option）

权限 core_site.xml

```
<property>
<name>hadoop.proxyuser.sdc.hosts</name>
<value>*</value>
</property>
<property>
<name>hadoop.proxyuser.sdc.groups</name>
<value>*</value>
</property>
```


HIVE_HOME/bin/hiveserver2

#### 外部 avro 表

```
CREATE external TABLE supplyEndpointOrder
  ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.avro.AvroSerDe'
  STORED AS INPUTFORMAT 'org.apache.hadoop.hive.ql.io.avro.AvroContainerInputFormat'
  OUTPUTFORMAT 'org.apache.hadoop.hive.ql.io.avro.AvroContainerOutputFormat'
  LOCATION 'hdfs:///gobblin-kafka/job-output/supplyEndpointOrder'
  TBLPROPERTIES ('avro.schema.url'='hdfs:///gobblin-kafka/job-output/supplyEndpointOrder.avsc');

  CREATE external TABLE supplyEndpointOrder-0-4
    ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.avro.AvroSerDe'
    STORED AS INPUTFORMAT 'org.apache.hadoop.hive.ql.io.avro.AvroContainerInputFormat'
    OUTPUTFORMAT 'org.apache.hadoop.hive.ql.io.avro.AvroContainerOutputFormat'
    LOCATION 'hdfs:///gobblin-kafka/job-output/supplyEndpointOrder'
    TBLPROPERTIES ('avro.schema.url'='hdfs:///gobblin-kafka/job-output/supplyEndpointOrder.avsc');


create table s_sales_day partitioned by (year string, month string , day2 string , hour string)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.avro.AvroSerDe'
STORED AS INPUTFORMAT 'org.apache.hadoop.hive.ql.io.avro.AvroContainerInputFormat'
OUTPUTFORMAT 'org.apache.hadoop.hive.ql.io.avro.AvroContainerOutputFormat'
LOCATION 'hdfs:///gobblin-kafka/job-output/staticSalesDay'
TBLPROPERTIES ('avro.schema.url'='hdfs:///gobblin-kafka/job-output/staticSalesDay.avsc');


insert overwrite  table s_sales_day partition(year='2017',month='04',day2='07',hour='11') select * from default.s_sales_day;

ALTER TABLE table s_sales_day ADD PARTITION (year='2017',month='04',day2='07',hour='12') location '/gobblin-kafka/job-output/staticSalesDay/2017/04/07/12'

```


#### 计算




create table hello( endpointOrderid string  ,endpointId string , totalmoney string , createtime string ,updatetime string) partitioned by (ds string);

INSERT OVERWRITE TABLE hello PARTITION (ds)
select endpointOrderid ,endpointId, totalmoney, regexp_extract(createtime,".*-.*-.* ",0) as ds, createtime, updatetime  from supplyendpointOrder;


```

create table s_sales_day( num  string  , totalmoney string , day string ) ;


按天计算销量和数量
INSERT OVERWRITE TABLE s_sales_day
select count(endpointorderid), sum(totalmoney) , ds  from (select endpointorderid ,totalmoney , updatetime ,regexp_extract(updatetime,".*-.*-[0-9]*",0) as ds ,  row_number() over (partition by endpointorderid order by updatetime desc ) num from supplyEndpointOrder_0_2 where updatetime> "2017-03-01 00:00:00" )  t where t.num =1 group by ds



hive> dfs -mv  /gobblin-kafka/job-output/staticSalesDay/2017/04/07/12/000000_0  /gobblin-kafka/job-output/staticSalesDay/2017/04/07/12/000000_0.avro

```


## hive server2

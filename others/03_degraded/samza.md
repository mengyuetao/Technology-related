####debug

task.opts=-agentlib:jdwp=transport=dt_socket,address=localhost:9009,server=y,suspend=y




####change offset

export JAVA_HOME='/c/PROGRA~1/Java/jdk1.8.0_31'
bin/checkpoint-tool.sh --config-path=file://C:/Users/test/Documents/03_devl/youchu/youchu_order/order/jpcf_order/jpcf_order_task/src/main/config/wikipedia-parser.properties  --new-offsets=file://C:/Users/test/Documents/03_devl/youchu/youchu_order/order/jpcf_order/jpcf_order_task/src/main/config/offset.properties
bin/checkpoint-tool.sh --config-path=file://C:/Users/test/Documents/03_devl/youchu/youchu_order/order/jpcf_order/jpcf_order_task/src/main/config/wikipedia-parser.properties


`offset.properties`

```
#tasknames.Partition\ 0.systems.kafka.streams.maxwell.partitions.0=7900
tasknames.Partition\ 0.systems.kafka.streams.ManageDtCoreEndpoint.partitions.0=0
#tasknames.Partition\ 0.systems.kafka.streams.order_endpointOrder.partitions.0=0
```


#### debug in eclipse

program arg
--config-factory=org.apache.samza.config.factories.PropertiesConfigFactory --config-path=file://C:/Users/test/Documents/03_devl/youchu/youchu_order/order/jpcf_order/jpcf_order_task/src/main/config/wikipedia-parser.properties  

vm arg
-Dlog4j.configuration=file:C:/Users/test/Documents/01_learn/hello-samza/deploy/samza/bin/log4j-console.xml

#### maxwell
max-well
bin/maxwell    --user='maxwell' --password='XXXXXX' --host='192.168.2.10' --producer=kafka --kafka.bootstrap.servers=192.168.2.30:9092   --include_tables=supply_accept_order,supply_endpoint_order  --include_dbs=jpcf


## 启动
SAMZA_PATH=/opt/samza/bin
$SAMZA_PATH/run-job.sh --config-factory=org.apache.samza.config.factories.PropertiesConfigFactory --config-path=../../install/samza_config/job_config/MaxwellAvroConvertDispatchTask.properties

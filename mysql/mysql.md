###

创建用户
```
 create user 'nlp'@'%' identified by 'MyNewPass4!'
 GRANT  ALL  ON  *.*  TO  'nlp'@'%'
 FLUSH PRIVILEGES
```

### 修改数据库 updateTime+1

SELECT * FROM `jpcf-82`.supply_endpoint_order  where create_time> '2016-10-15 07:25:16' order by endpoint_order_id desc;

UPDATE `jpcf-82`.`supply_endpoint_order` SET `update_time`=  date_add(str_to_date(update_time,'%Y-%m-%d %H:%i:%s'), interval 1 second) WHERE create_time> '2016-10-15 07:25:16' and endpoint_order_id>"" ;

select  date_add(str_to_date(update_time,'%Y-%m-%d %H:%i:%s'), interval 1 second) FROM `jpcf-82`.supply_endpoint_order where endpoint_order_id="20161019134156035552";


UPDATE `jpcf-82`.`supply_endpoint_order` SET `update_time`=date_add(`update_time`, interval 1 second) WHERE `endpoint_order_id`='20161103133307405729';


修改数据库，生成测试数据，下面模板

    UPDATE `core_product` set `update_time`=date_add(`update_time`, interval 1 second) where `product_id`>0;

#### 取子串

```
SELECT info,SUBSTRING(info,LOCATE('count',info)+7,LOCATE('}',info,LOCATE('count',info))-LOCATE('count',info)-7) as x FROM jpcf_report_82.report_endpoint where `key` like 'endpointOrder|day\_%';
```


查询数量
```
select y.info, y.x from (
SELECT info,SUBSTRING(info,LOCATE('count',info)+7,LOCATE('}',info,LOCATE('count',info))-LOCATE('count',info)-7) as x FROM jpcf_report_82.report_endpoint where `key` like 'endpointOrder|day\_%' ) as y where x > 100




## 权限

GRANT ALL ON test.* TO 'root'@'%';


## 日志
SHOW VARIABLES LIKE "general_log%"
;
set global general_log_file = "/data/mysql/nlpmysql.2.log" ;

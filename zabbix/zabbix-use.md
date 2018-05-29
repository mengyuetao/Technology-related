# Zabbix 使用

- Author: Yuetao Meng
- Mail: mfty1980@sina.com
- Date: 2018-5-29 v0.1


## zabbix 基本概念理解

通行标准中，大的网管系统一般有五大职责， 配置，性能，故障，计费，安全

大致理解
- zabbix 解决了性能，故障，部分安全？
- puppet 解决了配置，zabbix没有看到可以用snmp写入部分

总的来讲 zabbix 还是比较可扩展的，主要体现在 items 数据的收集上
items 原始数据被保存在数据库，趋势数据按小时计算后被保存到趋势数据库，由housekeeper保管。




items  包括主动收集，被动收集，计算值，日志类

- 主动被动收集可以用作，内存，磁盘，cpu的监控
- 日志是被动收集，具体场景，如监控mysql的慢查询日志
- 计算值从现有的数据取出，无需agent，如每小时慢查询日志数量


还有一种有趣的数据上报方式

```
zabbix_sender ­z zabbix ­s LinuxDB3 ­k db.connections ­o 43
```
其他标准方法， ssh，snmp，icpm 不再多讲，直接看官方文档

- 重要的事情：不要忘了配置每一个agent的host.name必须和添加的主机一样。否则主动收集指标会有问题


## 有用的命令

收集数据

```
zabbix_get ­s 127.0.0.1 ­k system.cpu.load
zabbix_agentd ­t "vfs.file.regexp[/etc/passwd,root]"
```

有问题解决不了的时候，看看agent日志定位问题
/tmp/zabbix_agentd.log




## 使用进阶一，观测 items

Monitoring -> latest data


## inventory

这个功能主要是提供资产管理，
启用的话在配置中，配置自动inventory，zabbix会自动添加设备和资产

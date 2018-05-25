# Kafka replication 分析

- Author: Yuetao Meng
- Mail: mfty1980@sina.com
- Date: 2018-5-26 v0.1


## 概述

- 本文用于理解kafka冗余设计
- kafka与zookeepr协作实现冗余，是 zookeeper 的场景应用很好的案例

高层设计：
1. kafka 冗余的粒度是 Topic
2. kafka 冗余使用 leader 和 fellows
3. zookeeper 起到的比较大的作用
4. 所有的 brokers 中有一个 为 controller 负责 计算每个topic的 leader 和 fellows

## 实现


1. broker 集群， 通过 zookeeper 全局锁 确定自己是否为 controller
2. controller 通过 zookeeper 发现 所有的broker，并监控broker的状态， 同时 topic 所在的 leader 和 fellows 的分配计算
3. controller 通过 rpc 接口，与broker 通行，执行 topic的 leader 和 fellows 的创建
4. controller 选择算法 基于  ISR （in-sync-replication）， AR （all replication） （具体描述见下述参考文档）  
5. 管理端的配置发送到指定的 zookeeper 节点，contorller 发现配置更新，通知broker 创建新的topic或删除topic
6. 客户端通过 zookeeper 发现 每个topic的leader，ISR 用于连接


## 主要场景分析

A. Failover during broker failure.
B. Creating/deleting topics.
C. Broker acts on commands from the controller.
D. Handling controller failure.
E. Broker startup.
F. Replica reassignment
G. Follower fetching from leader and leader advances HW
H. Add/remove partitions to an existing topic

## 主要场景分析参考

[kafka Detailed Replication Design V3](https://cwiki.apache.org/confluence/display/KAFKA/kafka+Detailed+Replication+Design+V3)

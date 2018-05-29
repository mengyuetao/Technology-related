# Enabling Exactly-Once in Kafka Streams

- Author: Yuetao Meng
- Mail: mfty1980@sina.com
- Date: 2018-5-29 v0.1


 ## 概述

流处理系统中，特别是分布的流处理系统，需要确保消息被处理且被处理一次。否则可能导致计算结果的错误或不一致。

主要考虑几个点：

1. 当前输入topic的位置     (保存在topic)
2. 当前计算的状态（数据库）  (保存在topic)
3. 发出的消息一致性         (保存在topic)

可见，所有实现都是使用 kafka 时候，kafka broker 实现了事务能力，即初始化事务，提交和cancel
所以，只要使用kafkastream 内部的实现，可以确保处理一次原语
如果使用了外部数据库，调用外部接口，目前应该还不支持，需要在应用层面保证，使用至少一次的原语

所以kafkastream使用的场景是有约束的，主要用于数据计算，不建议调用外部接口，因为不能保证重复数据。

顺便记录一下，kafka 事务实现，在broker中选举一个 事务 controller ，具体看下文。 原理和topic controler应该是一样的。


 ## 参考
[Enabling Exactly-Once in Kafka Streams](https://www.confluent.io/blog/enabling-exactly-kafka-streams/)

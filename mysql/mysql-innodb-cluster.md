
# mysql innodb cluster

- Author: Yuetao Meng
- Mail: mfty1980@sina.com
- Date: 2018-5-30


## 概述

大家知道，mysql 5.7 除了一个杀手级别的功能，那就是基于 MySQL group replication 的 mysql innodb cluster，弥补了mysql 可用性短板，在不切换innodb的好处上，彻底的解决了
mysql 的可用性问题 ，下面讲一下原理，供技术参考。

## multi-paxos 实现分析（坑）

1. prepare  要求对方承诺 不接受小于n的提案
2. accept   要收接受不小于n的提暗的值，如发现大于n，转到1
3. lean     通知其他成员结果

lean 主要用于在leader挂了的时候，其他成员接替并提案 nop

## 具体实现

1. 采用了 现有的 binlog， 加上 half sync 机制，binlog 需要对端确认
2. 集群执行的时候，加入了certfy，如果certiy必须先成功，我的理解是，集群的前提是有序的，即有编号的，
    应提交事务在自己的 slot上，所以真个集群的状态只有 当前slot能改变，
    只要 certity成功了，就代表 日志同步过去后不会产生冲突，应为其他成员也会做certify，这样就不会有问题了。

> certify的理解，打个比方和开盘摇号一样，为了避免一套房被多个人那，有100个房子，由一个人来分，和发号码，假如10个人要买，每个人需要按次序拿号，号用完了可以再拿，但是你拿的号必须是上一次的号 +10 ， 如第一个人只能拿到 1，11，21， 第二个人拿到2，12, 那么买房就很简单， certify 就是做两件事情，第一 检查你手上的票编号，和看病一样，先来先拿，中间跳号了你就需要等，轮到你的时候，你选房，不选那你就只能放弃，号也用掉了，下一个人继续拿



## 参考资料本地拷贝

1. [Group Replication: A Journey to the Group Communication Core](mysql/mysqlinnodbclusterandgroupreplicationinanutshell-hands-ontutorialwithmeb-170523203520.pdf)
2. [MySQL innodb cluster and Group Replication in a nutshell - hands-on tutorial with MySQL Enterprise Backup](mysql/fosdemgrpaxos-170207060213.pdf)


## 参考资料原始链接


1. [“The king is dead, long live the king”: Our Paxos-based consensus](http://mysqlhighavailability.com/the-king-is-dead-long-live-the-king-our-homegrown-paxos-based-consensus/)
2. [Group Replication: A Journey to the Group Communication Core](https://www.slideshare.net/alfranio1/group-replication-a-journey-to-the-group-communication-core-71845289?from_action=save)
3. [MySQL innodb cluster and Group Replication in a nutshell - hands-on tutorial with MySQL Enterprise Backup](https://www.slideshare.net/lefred.descamps/mysql-innodb-cluster-and-group-replication-in-a-nutshell-handson-tutorial-with-mysql-enterprise-backup?qid=6060b453-3e48-445d-857b-1edbb1d82366&v=&b=&from_search=7)

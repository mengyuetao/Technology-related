
# Mysql Innodb Cluster  

- 作者: 孟越涛 Yuetao Meng
- Mail: mfty1980@sina.com
- Date: 2018-5-30


## 概述

Mysql 5.7 关键特性group replication，算是解决了数据库系统可用性问题。
所谓可用性，就是万一系统挂了，你的系统就中止服务了。

那么，它是如何做到的呢？下面先从 paxos说起：

## paxos

1. prepare  要求对方承诺 不接受小于n的提案
2. accept   发起接受不小于n的提案的值，如发现大于n，转到1
3. learn    accept成功后，通知其他成员结果

learn 主要用于在leader挂了的时候，其他成员接替并提案 nop

> 这里还不是很理解的话，看下面参考的资料

## 具体实现

1. 采用了现有的 binlog， 需要 half sync 机制，就是每次提交的时候，binlog 需要对端确认，以前的实现是没有这一步的。
2. 每次提交事务的时候，还需要majoriy（英语翻译多数人）通过certify，certify 必须成功，certify可行是由于系统架构基于multi-paxos的有序更新，
    每个事务提交都在自己的slot（槽位）上，每个slot都是有序的，所以整个集群的状态只会被当前slot改变，只要majoriy都通过certify，那么就可以安全提交事务了，
   ，不会有写冲突，日志同步过去后不会产生冲突，因为其他成员也会做certify，这样就不会有问题了，但是其他成员会出现并发写冲突，采用乐观锁定解决。
3. 乐观锁定，certify的时候，万一发现冲突，本地事务失败，如果成功事务提交。

> 理解：拿购房摇号打个比方，把每次选房卖房成功看做是一次数据库成功事务提交，假如现场只有一个销售，那么很好办，按拿号顺序就可以了，但是现场1个销售处理事情
实在是太慢了，所以就安排了有多个销售人员负责销售，那么问题来了，一套房有可能被多人同时选中（一个销售看作是集群中一台节点）产生重复购买，为了避免一套房重复买，销售员之间需要协商一个销售规则（certify +  half sync），规则这样执行，假设有100套房子，购房者先排队，然后由政府发号，每个号都是连续的，购房者按照排队次序拿号，拿到号按大到小顺序选房， 随机选一个空闲的销售员办理， 这时销售做两件事情（certify），第一 检查你手上的号，然后看一下前面所有销售最后的已经处理完的编号，如果还没有轮到你就需要等，等到轮到你的时候你选房或放弃， 那么问题来了，如果前面的人一直不出现呢，那么销售就会提案，通知半数以上的销售废弃前面这个号，如果没有销售反对的话，就把前面的人的票无效掉，然后为下一个人办理，如果有人反对，说明那个人已经在其他销售那边办了，只是那个销售没有被通知到（后面会讲到只通半数以上销售就可以了，因为全部通知太费时间，所以他有可能没有通知到，这个过程叫做learn），接下来就可以给你办理了，不管你最终办理成功后还是没有成功，都会通知半数以上的销售。 这里还有一个问题，如果正在办的人选了同一套房怎么办， 那就是certify的乐观锁定， 虽然你已经选办好房，最后发现别的销售那里同时有人选了，同时你的号比较靠后，那么只能重新选了。 当然实际情况还要复杂，比如有的销售下班了。



## 参考资料本地拷贝

1. [Group Replication: A Journey to the Group Communication Core](mysql/mysqlinnodbclusterandgroupreplicationinanutshell-hands-ontutorialwithmeb-170523203520.pdf)
2. [MySQL innodb cluster and Group Replication in a nutshell - hands-on tutorial with MySQL Enterprise Backup](mysql/fosdemgrpaxos-170207060213.pdf)


## 参考资料原始链接


1. [“The king is dead, long live the king”: Our Paxos-based consensus](http://mysqlhighavailability.com/the-king-is-dead-long-live-the-king-our-homegrown-paxos-based-consensus/)
2. [Group Replication: A Journey to the Group Communication Core](https://www.slideshare.net/alfranio1/group-replication-a-journey-to-the-group-communication-core-71845289?from_action=save)
3. [MySQL innodb cluster and Group Replication in a nutshell - hands-on tutorial with MySQL Enterprise Backup](https://www.slideshare.net/lefred.descamps/mysql-innodb-cluster-and-group-replication-in-a-nutshell-handson-tutorial-with-mysql-enterprise-backup?qid=6060b453-3e48-445d-857b-1edbb1d82366&v=&b=&from_search=7)

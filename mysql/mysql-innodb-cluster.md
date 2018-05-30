
# Mysql Innodb Cluster

- 作者: 孟越涛 Yuetao Meng
- Mail: mfty1980@sina.com
- Date: 2018-5-30


## 概述

Mysql 5.7 关键特性， MySQL group replication，很好的解决了数据库系统，可用性问题。
所谓可用性，就是万一系统挂了，你就中止服务了。

那么，他是如何做到的呢？下面先从 paxos说起：

## paxos

1. prepare  要求对方承诺 不接受小于n的提案
2. accept   要收接受不小于n的提暗的值，如发现大于n，转到1
3. learn     通知其他成员结果

learn 主要用于在leader挂了的时候，其他成员接替并提案 nop

> 这里还不是很理解的话，找相关资料再看

## 具体实现

1. 采用了现有的 binlog， 加上 half sync 机制，就是每次提交的时候，binlog 需要对端确认
2. 每次提交的时候，还需要majoriy（英语翻译多数人）通过certify，certify 必须成功，certify可行是由于系统架构基于multi-paxos的有序更新，
    每个事务提交都在自己的slot上，每个slot都是有序的，所以整个集群的状态只会被当前slot能改变，只要majoriy通过certify，那么就可以安全提交更新了，
   ，不会有写冲突，日志同步过去后不会产生冲突，应为其他成员也会做certify，这样就不会有问题了。
3. 本地乐观锁定，certify的时候，万一发现冲突，本地事务失败，如果成功事务提交。

> certify的理解，打个比方和开盘摇号一样，为了避免一套房被多个人拿，需要又一个选房顺序，假设有100套房子，购房者先申请，到截止时间，人数就不能再加了，然后由政府发号，假如共截止时间一共10个人，按照排队次序，每个人需要按次序拿号，号用完了可以再拿第二轮，但是你拿的号必须按排队次序 ， 假设中间没有人退出，第一个人可以拿到 1，11，21， 第二个人拿到2，12,.. 那么买房就很简单， certify 就是做两件事情，第一 检查你手上的票编号，和看病一样，先来先选房，中间跳号了你就需要等，轮到你的时候，你选房，不选那你可以放弃，但是本轮的号也用掉了，下一个人继续选，这样，不会有人拿到同一套房了，你不买的话，下面的人还可以买。 但是这回有一个问题，后面的人要等，那么开发商想出了一个办法，反正销售都空着，那么就可以给你提前办，那就是certify的乐观锁定，你前面还排了3个人，还剩下97套房，那么销售就告诉你，你先选吧，我给你先办手续，这样的话轮到你的时候就已经办好了，你就不用等了，但是前提是要看看你选的房前面三个人有没有人选，选了就白办了，需要重新选，所以 certify 作用就是在这里，虽然你提前办好了，如果运气差的话你还是买不到你选的房，但是运气好的话，那么就可以提前办好了，坐着喝喝咖啡等结果就是，这个销售过程也快了不少。



## 参考资料本地拷贝

1. [Group Replication: A Journey to the Group Communication Core](mysql/mysqlinnodbclusterandgroupreplicationinanutshell-hands-ontutorialwithmeb-170523203520.pdf)
2. [MySQL innodb cluster and Group Replication in a nutshell - hands-on tutorial with MySQL Enterprise Backup](mysql/fosdemgrpaxos-170207060213.pdf)


## 参考资料原始链接


1. [“The king is dead, long live the king”: Our Paxos-based consensus](http://mysqlhighavailability.com/the-king-is-dead-long-live-the-king-our-homegrown-paxos-based-consensus/)
2. [Group Replication: A Journey to the Group Communication Core](https://www.slideshare.net/alfranio1/group-replication-a-journey-to-the-group-communication-core-71845289?from_action=save)
3. [MySQL innodb cluster and Group Replication in a nutshell - hands-on tutorial with MySQL Enterprise Backup](https://www.slideshare.net/lefred.descamps/mysql-innodb-cluster-and-group-replication-in-a-nutshell-handson-tutorial-with-mysql-enterprise-backup?qid=6060b453-3e48-445d-857b-1edbb1d82366&v=&b=&from_search=7)



## zookeeper

选举投票，参考下文

[Zookeeper架构及FastLeaderElection机制](http://www.jasongj.com/zookeeper/fastleaderelection/)

这里有几个疑惑，zookeepr 最难理解的应该是选主了，应该先理解一个paxos后会比较容易。

1 理解：投票算法和 paxos 的关系：可以理解为multi-paxos，逻辑上，multi求的的是一个值，发起的消息，与回复重复，所以省下了回复消息。
2 猜测：加入有1，2，3，4，5 台服务器，1，2，3 协商完成，3为leader，此时，4，5 拉拢了1，5为leader，那么3连接2成功，连接1不成功，数量小于3，应该会发起下一轮的选举？
3 读数据，不能保证最新，和 mysql innodb 一样 cluster，读数据是有延迟的，需要用sync同步一下。



具体应用：

1 配置
2 全局锁，使用内部排序号。： 这里还是有一个问题，如果客户端和zookeeper断开，那么应该停止工作。还是比较复杂哦。

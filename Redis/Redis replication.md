## redis replication 的核心机制

- redis 采用异步方式复制数据到 slave 节点

  不过 redis 2.8 开始，slave node 会周期性地确认自己每次复制的数据量

- 一个 master node 是可以配置多个 slave node 的

- slave node 也可以连接其他的 slave node

- slave node 做复制的时候，是不会 block master node 的正常工作的

- slave node 在做复制的时候，也不会 block 对自己的查询操作

  它会用旧的数据集来提供服务; 但是复制完成的时候，需要删除旧数据集，加载新数据集，这个时候就会暂停对外服务了

- slave node 主要用来进行横向扩容，做读写分离，扩容的 slave node 可以提高读的吞吐量

实现高可用性 slave 有很大的关系，后面会讲解

## master 持久化对于主从架构的安全保障的意义

如果采用了主从架构，那么建议必须开启 master node 的持久化！

很简单的道理，master 提供写，它自己的数据是最完整的，所以需要它自己来做持久化。

如果不使用 master 做持久化的冷备，而采用 slave 来做冷备的话，当 master 死机再重启，因为自己本地没有数据，会将空的数据同步到所有的 slave 上去。

其次 master 的各种备份方案，要不要做，万一说本地的所有文件丢失了; 从备份中挑选一份 rdb 去恢复 master; 这样才能确保 master 启动的时候，是有数据的

后面会讲解哨兵（sentinal）高可用机制，即使采用了该高可用机制，slave node 可以自动接管 master node，但是也可能 sentinal 还没有检测到 master failure，master node 就自动重启了，还是可能导致上面的所有 slave node 数据清空故障



参考：

[https://zq99299.github.io/note-book/cache-pdp/redis/015.html#redis-replication-%E7%9A%84%E6%A0%B8%E5%BF%83%E6%9C%BA%E5%88%B6](https://zq99299.github.io/note-book/cache-pdp/redis/015.html#redis-replication-的核心机制)
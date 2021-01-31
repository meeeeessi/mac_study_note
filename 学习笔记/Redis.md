# Redis

## 安装及配置redis

### 1.[redis官网下载](https://redis.io/download)

[redis中文网](http://redis.cn/)

### 2.将压缩包拷贝到Linux服务器中

**2.1具体步骤**

https://www.cnblogs.com/rgever/p/redis.html

**2.2过程中遇到的问题**

<font color='red'>ssh_init: Network error: Cannot assign requested address</font>

>  解决方法是加上端口号，如下所示:
>
>  pscg -P 22 tnsnames.ora root@192.168.50.5:/home

<font color='red'>pscp: unable to open /soft/: failure</font>

> 检查Linux文件路径

### 3.在Linux中解压redis压缩包

```shell
# tar -zxvf redis-6.0.9.tar.gz
```

### 4.Linux下redis部署

[相关文档](https://www.cnblogs.com/zdd-java/p/10288734.html)



## redis中的常用命令

### 1.redis命令参考

http://redisdoc.com/

### 2.笔记

bin目录下启动redis服务：./redis-server /usr/local/redis/etc/redis.conf

查看后台redis服务进程是否启动：ps -ef|grep redis

连接redis：./redis-cli -p 6379   或   redis-cli -p 6379 

检查redis是否连接成功：ping

查询数据库中数据大小：db＋tab键

查看数据库中所有数据：keys *

选择第八个数据库：select 7

删除所有数据库数据：FLUSHALL

删除当前库数据：FLUSHDB

**redis键（key）**

判断某个key是否存在：EXISTS key1

移动某个key到某个库：MOVE key3 2

查看还有多少秒过期，-1表示永不过期，-2表示已过期：ttl key1

给key设置过期时间（秒）：expire key2 10

查看key类型：type key1

拼接某key：append key1 12323343

查看某key长度：STRLEN key1

每次加1：INCR key2 

每次减1：DECR key2

每次加多个值：INCRBY key2 3

每次减多个值：DECRBY key2 2

获取指定区间的值：GETRANGE key1 0 5 

范围内覆盖（下标为0开始）：SETRANGE key1 0 wang

如果某key不存在设置值生效：SETNX key1 sasasa

**redis列表（List）**

同时设置多个值：mset k1 v1 k2 v2 k3 v3

同时获取多个值：mget k1 k2 k3

**redis集合（set）**

sadd添加一到多个数值：sadd set01 1 1 2 2 3 3

查看set中所有值：SMEMBERS set01

查看set中具体某个值：SISMEMBER set01 1

获取元素中的个数：SCARD set01

删除一到多个数值：SREM set01 1 2

某个set中随机抽取三（1到多个）个key：SRANDMEMBER set01 3

随机出栈：spop set01

将set1中的set某个值赋给set2：SMOVE set01 set02 4

**数学集合类**

差集（在第一个set里面而不在后面任何一个set里面的项）：SDIFF set01 set02

交集：SINTER set01 set02

并集：SUNION set01 set02

**redis哈希（Hash）**

KV模式不变，但V是一个键值对（user为key   id 11 为value）：

| set                                  | get                                         |
| ------------------------------------ | ------------------------------------------- |
| hset user id 11                      | hget user id                                |
| hmset customer id 11 name li4 age 37 | hmget customer id name age/hgetall customer |

删除某个key：hdel user name

查看key长度：hlen user

在key里面的某个值的key：HEXISTS customer id

获取所有的value：hvals customer

查看key中所有的key：HKEYS customer 

某key,整型value值递增：HINCRBY customer age 2

某key,浮点型value值递增：HINCRBYFLOAT customer score 0.5

age不存在即set，age存在不set：HSETNX customer age 26 

**集合Zset（sorted set）**

zadd添加一到多个数值：zadd zset01 60 v1 70 v2 80 v3 90 v4 100 v5

查看所有value：zrem zset01 v4

查看所有value及等级：ZRANGE zset01 0 -1 withscores

查看在区间内的value：ZRANGEBYSCORE zset01 60 90

在区间内不包含90的：ZRANGEBYSCORE zset01 60 (90

在区间内不包含60，90的：ZRANGEBYSCORE zset01 (60 (90

类似于分页（从哪截取几个）：ZRANGEBYSCORE zset01 60 90 limit 2 2 

删除元素：zrem zset01 v4

统计个数：ZCARD zset01

统计区间内的个数：ZCOUNT zset01 60 100

获取下标：ZRANK zset01 v2

获取score：ZSCORE zset01 v3

反方向下标：ZREVRANK zset01 v3

反方向显示：ZREVRANGE zset01 0 -1 

根据score反方向显示：ZREVRANGEBYSCORE zset01 90 60

**设置redis密码**

查看密码：config get required

设置密码：config set requirepass "123456"

登录：auth 123456

## Redis事务

### 1.命令

开启事务：MULTI

入队：将多个命令入队到事务中，接到这些命令并不会立即执行，而是放到等待执行的事务队列里面

提交事务：EXEC

放弃事务：DISCARD

### 2.watch监控

悲观锁/乐观锁/CAS(Check And Set)

悲观锁：操作数据库某表时，将整张表锁住，其他人不可以对该表进行操作（平时不推荐使用，但是数据库备份时可以使用）

乐观锁：可以多人对同一张表甚至同一条数据进行操作，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制（乐观锁适用于多读的应用类型，这样可以提高吞吐量）

<font color='red'>乐观锁策略：提交版本必须大于记录当前版本才能执行更新</font>

监控：WATCH key值 ... ...

放弃监控：UNWATCH

[redis的Watch机制是什么？](https://blog.csdn.net/qq_43371004/article/details/103439599)



## Redis持久化

### 1.rdb(Redis Database)

每次FLUSHALL和SHUTDOWN都会产生dump.rdb文件

### 2.aof(Append Only File)

修复aof文件命令：redis-check-aof --fix appendonly.aof

**redis.conf**：

appendonly-----默认为no，yes就打开aof持久化

appendfsync-----默认为everysec（always/everysec/no）[相关文档](https://www.cnblogs.com/aquester/p/10080991.html)

no-appendfsync-on-rewrite-----重写时是否可以运用appendfsync，默认为no，可保证数据安全

auto-aof-rewrite-min-size-----设置重写的基准值（已存redis数据大小）

auto-aof-rewrite-percentage-----设置重写的基准值

### 3.Redis的两种持久化RDB和AOF

[相关文档](https://blog.csdn.net/qq_36795474/article/details/82938721?utm_medium=distribute.pc_relevant_t0.none-task-blog-OPENSEARCH-1.control&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-OPENSEARCH-1.control)

### 4.RDB和AOF持久化对比

[相关文档](https://blog.csdn.net/huangtao99/article/details/50662255)

## Redis的发布订阅

<font color='red'>先订阅后发布才能收到消息</font>

ctrl+c退出

1.可以一次性订阅多个：SUBSCRIBE c1 c2 c3

2.消息发布：PUBLISH c2 hellokittle



3.订阅多个，通配符\*：PSUBSCRIBE new*

4.消息发布：PUBLISH new1 hellokittle



## Redis的复制（Master/Slave）

### 1.什么是redis的复制

就是我们所说的主从复制，主机数据更新后根据配置和策略，自动同步到备机的master/slaver机制，master以写为主，slaver以读为主

该端口号的查看复制信息：info replication

设置该端口号的主端口号：SLAVEOF 127.0.0.1 6379

### 2.复制的原理

slave启动成功连接到master后会发送一个sync（复制）命令

master接到命令启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，在后台进程执行完毕后，master将传送整个数据文件到slave，以完成一次完全同步

全量复制：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。

增量复制：master继续将新的所有收集到的修改命令依次传给slave，完成同步，但是只要是重新连接master，一次完全同步（全量复制）将被自动执行



## Redis的哨兵模式（sentinel）

### 1.命令

在sentinel.conf文件中配置哨兵服务：sentinel monitor host6379 127.0.0.1 6379 1

启动哨兵服务（bin目录下）：./redis-server /usr/local/redis/etc/sentinel.conf --sentinel

查看复制信息：info replication



## win10启动redis中遇到的错误

Could not connect to Redis at 127.0.0.1:6379: 由于目标计算机积极拒绝,无法连

[解决方法](https://blog.csdn.net/shang_0122/article/details/106340489)

redis-server.exe redis.windows.conf

redis-cli.exe -h 127.0.0.1 -p 6379

### creating server tcp listening socket 127.0.0.1:6379: bind No error

[解决方法](https://blog.csdn.net/n_fly/article/details/52692480?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-3.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-3.control)

## 关于CAP

https://blog.csdn.net/yeyazhishang/article/details/80758354

Consistency (一致性)：

“all nodes see the same data at the same time”,即更新操作成功并返回客户端后，所有节点在同一时间的数据完全一致，这就是分布式的一致性。一致性的问题在并发系统中不可避免，对于客户端来说，一致性指的是并发访问时更新过的数据如何获取的问题。从服务端来看，则是更新如何复制分布到整个系统，以保证数据最终一致。

Availability (可用性):

可用性指“Reads and writes always succeed”，即服务一直可用，而且是正常响应时间。好的可用性主要是指系统能够很好的为用户服务，不出现用户操作失败或者访问超时等用户体验不好的情况。

Partition Tolerance (分区容错性):

即分布式系统在遇到某节点或网络分区故障的时候，仍然能够对外提供满足一致性或可用性的服务。

分区容错性要求能够使应用虽然是一个分布式系统，而看上去却好像是在一个可以运转正常的整体。比如现在的分布式系统中有某一个或者几个机器宕掉了，其他剩下的机器还能够正常运转满足系统需求，对于用户而言并没有什么体验上的影响。
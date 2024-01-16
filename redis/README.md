---
description: redis
---

# redis

## 使用跳表而不使用红黑树原因&#x20;

* zset 需要经常执行zrange 和zrevrange命令，跳表的缓存局部性至少和其他类型的平衡树一样好
* 平衡树插入删除可能引起子树的调整，跳表的插入删除只需要修改相邻指针。
* 跳表更容易调试，实现。

## 过期删除策略&#x20;

* 惰性删除
* 定期删除。

## 内存淘汰策略&#x20;

* 通过在redis.conf maxmemory 参数设置最大内存，不设置默认是不限制的。一般设置其为物理内存的3/4
* noeviction：直接返回错误，不淘汰任何已经存在的redis键 (默认策略)
* allkeys-lru：所有的键使用lru算法进行淘汰
* volatile-lru：有过期时间的使用lru算法进行淘汰
* allkeys-random：随机删除redis键
* volatile-random：随机删除有过期时间的redis键
* volatile-ttl：删除快过期的redis键
* volatile-lfu：根据lfu算法从有过期时间的键删除
* allkeys-lfu：根据lfu算法从所有键删除

##

## 同时配置了rdb和aof&#x20;

* 取aof 的数据

## RDB优缺点&#x20;

* 大数据量时，恢复效率比AOF更高
* 更节省磁盘空间\
  缺点：
* 会导致数据丢失
* 数据量足够大时，fork子进程持久化可能导致服务器停止一段时间



## redis 事务&#x20;

* watch 如果事务在执行前，key被其他命令修改，则事务中止
* multi 开启事务
* exec 执行事务
* discard 取消事务









缓存击穿 [#flashcard](app://obsidian.md/index.html#flashcard)\
热点数据，缓存不在，但是数据库存在。\
解决：

* 设置热点数据永不过期
* 接口限流，熔断，降级
* 加上互斥锁



redis 主从数据全量同步 [#flashcard](app://obsidian.md/index.html#flashcard)

* 第一阶段是主从库间建立连接、协商同步的过程，主要是为全量复制做准备。在这一步，从库和主库建立起连接，并告诉主库即将进行同步，主库确认回复后，主从库间就可以开始同步了。具体来说，从库给主库发送 psync 命令，表示要进行数据同步，主库根据这个命令的参数来启动复制。psync 命令包含了主库的 runID 和复制进度 offset 两个参数。runID，是每个 Redis 实例启动时都会自动生成的一个随机 ID，用来唯一标记这个实例。当从库和主库第一次复制时，因为不知道主库的 runID，所以将 runID 设为“？”。offset，此时设为 -1，表示第一次复制。主库收到 psync 命令后，会用 FULLRESYNC 响应命令带上两个参数：主库 runID 和主库目前的复制进度 offset，返回给从库。从库收到响应后，会记录下这两个参数。这里有个地方需要注意，FULLRESYNC 响应表示第一次复制采用的全量复制，也就是说，主库会把当前所有的数据都复制给从库。
* 第二阶段，主库将所有数据同步给从库。从库收到数据后，在本地完成数据加载。这个过程依赖于内存快照生成的 RDB 文件。具体来说，主库执行 bgsave 命令，生成 RDB 文件，接着将文件发给从库。从库接收到 RDB 文件后，会先清空当前数据库，然后加载 RDB 文件。这是因为从库在通过 replicaof 命令开始和主库同步前，可能保存了其他数据。为了避免之前数据的影响，从库需要先把当前数据库清空。在主库将数据同步给从库的过程中，主库不会被阻塞，仍然可以正常接收请求。否则，Redis 的服务就被中断了。但是，这些请求中的写操作并没有记录到刚刚生成的 RDB 文件中。为了保证主从库的数据一致性，主库会在内存中用专门的 replication buffer，记录 RDB 文件生成后收到的所有写操作。
* 第三个阶段，主库会把第二阶段执行过程中新收到的写命令，再发送给从库。具体的操作是，当主库完成 RDB 文件发送后，就会把此时 replication buffer 中的修改操作发给从库，从库再重新执行这些操作。这样一来，主从库就实现同步了。
* ![image-20230130112813322](https://cdn.jsdelivr.net/gh/lwx-fly/img@master/img/image-20230130112813322.png)

redis 对象类型 [#flashcard](app://obsidian.md/index.html#flashcard)

* type记录了对象所保存的值的类型，它的值可能是以下常量中的一个：
* define OBJ\_STRING 0 // 字符串
* define OBJ\_LIST 1 // 列表
* define OBJ\_SET 2 // 集合
* define OBJ\_ZSET 3 // 有序集
* define OBJ\_HASH 4 // 哈希表
* **encoding记录了对象所保存的值的编码**，它的值可能是以下常量中的一个：
* define OBJ\_ENCODING\_RAW 0 / _Raw representation_ /
* define OBJ\_ENCODING\_INT 1 / _Encoded as integer_ /
* define OBJ\_ENCODING\_HT 2 / _Encoded as hash table_ /
* define OBJ\_ENCODING\_ZIPMAP 3 / _注意：版本2.6后不再使用._ /
* define OBJ\_ENCODING\_LINKEDLIST 4 / _注意：不再使用了，旧版本2.x中String的底层之一._ /
* define OBJ\_ENCODING\_ZIPLIST 5 / _Encoded as ziplist_ /
* define OBJ\_ENCODING\_INTSET 6 / _Encoded as intset_ /
* define OBJ\_ENCODING\_SKIPLIST 7 / _Encoded as skiplist_ /
* define OBJ\_ENCODING\_EMBSTR 8 / _Embedded sds string encoding_ /
* define OBJ\_ENCODING\_QUICKLIST 9 / _Encoded as linked list of ziplists_ /
* define OBJ\_ENCODING\_STREAM 10 / _Encoded as a radix tree of listpacks_ /
* ptr是一个指针，指向实际保存值的数据结构
* lru属性: 记录了对象最后一次被命令程序访问的时间

redis 执行数据类型命令流程 [#flashcard](app://obsidian.md/index.html#flashcard)

* 根据给定的key，在数据库字典中查找和他相对应的redisObject，如果没找到，就返回NULL；
* 检查redisObject的type属性和执行命令所需的类型是否相符，如果不相符，返回类型错误；
* 根据redisObject的encoding属性所指定的编码，选择合适的操作函数来处理底层的数据结构；
* 返回数据结构的操作结果作为命令的返回值。



redis 缓冲区 [#flashcard](app://obsidian.md/index.html#flashcard)

* repl\_backlog\_buffer 适用于主从同步时候，主从链接断开了，恢复链接后，从库会发送slave\_repl\_offset给主库，主库根据这个来判断是要进行增量同步还是全量同步。所有从库共用一个repl\_backlog\_buffer
* replication\_buffer。主库和每个从库之间都会分配replication buffer来进行通信。

# Redis学习过程

#### redis 官方文档 redis.io/documentation

**************************

## 一、各种命令

切换数据库 ```select [index]```

添加或修改键 ```set 变量名```

批量添加键 ```MSET [键名] [值] [键名] [值] [键名] [值]``` ***仅限String类型***

添加或修改哈希键 ```HSET [键] [field] [值]``` ***仅限Hash类型***

批量添加哈希键 ```HMSET [键] [field] [值] [field] [值] [field] [值]``` ***仅限Hash类型***

获取键 ```get 变量名```

批量获取键 ```MGET [键名] [键名] [键名] ``` ***返回结果为数组***

获取哈希键 ```HGET [键]``` ***仅限Hash类型***

批量获取哈希键 ```HMGET [键名] [field] [field] [field]``` ***返回结果为数组***

获取哈希键的全部字段和值 ```HGETALL [键]```

获取哈希键的全部字段 ```HKEYS [键]```

获取哈希键的全部值 ```HVALS [键]```

查询键 ```KEYS [想查询的变量名]```

查询所有键 ```KEYS *```

删除键 ```DEL [键名]```

删除多个键 ```DEL [键名] [键名] [键名]```

判断键是否存在 ```EXISTS [键名]```

设置键的有效期 ```EXPIRE [键名] [有效期] #有效期单位为秒```

查看有效期 ```TTL [键名] #返回结果中，-2为过期， -1为永久```

键的值自增1 ```INCR [键]``` ***仅限整型***

键的整型值增长指定数 ```INCRBY [键] [整型数]``` ***可输入负数***

键的浮点数值增长指定数 ```INCRBYFLOAT [键] [浮点数]``` 

哈希键的值增长指定数 ```HINCRBY [键] [字段] [数]```

添加键 ```setnx [键] [值]``` ***已存在则不新增***

添加哈希键 ```setnx [键] [字段]``` ***字段已存在则不新增***

添加键且设置有效期 ```setex [键] [有效期] [值]``` 或 ```set [键] [值] ex [有效期]```

从左添加列表元素 ```LPUSH [键] [元素1] [元素2] [元素3]```

从右添加列表元素 ```RPUSH [键] [元素1] [元素2] [元素3]```

从左获取列表元素 ```LPOP [键] [获取元素的数量]```

从右获取列表元素 ```RPOP [键] [获取元素的数量]```

获取指定片段的元素 ```LRANGE [键] [片段起始序号] [片段结束序号]```

阻塞式从左获取列表元素 ```BLPOP [键] [等待时间]```

阻塞式从右获取列表元素 ```BRPOP [键] [等待时间]```

向集合中添加元素 SADD ```[键] [元素] [元素] [元素]```

查看集合中的所有元素 ```SMEMBERS [键]```

从集合中删除元素 ```SISMEMBER [键] [元素]```

查看集合中的元素个数 ```SCARD [键]```

合并两个集合 ```SUNION [键1] [键2]```

求两个集合的交集 ```SINTER [键1] [键2]```

求两个集合的差集 ```SDIFF [键1] [键2]```

向排序集中添加元素 ```ZADD [键] [值] [字段] [值] [字段]```

删除排序集中的元素 ```ZREM [键] [字段]```

获取排序集中元素的排名 ```ZRANK [键] [字段]```

获取排序集中元素的倒序排名 ```ZRERANK [键] [字段]```

查询排序集中的元素个数 ```ZCARD [键]```

统计排序集中指定分段的元素个数 ```ZCOUNT [键] [起始分数] [结束分数]``` ***返回数量***

给排序集中的指定元素增加分数 ```ZINCRBY [键] [分数] [字段]```

查询排序集中指定一段序号的元素 ```ZRANGE [键] [排序起始序号] [排序结束序号]``` ***返回元素***

查询排序集中指定分段的元素 ```ZRANGEBYSCORE [键] [起始分数] [结束分数]``` ***返回元素***


## 二、Redis数据结构介绍

### 1 基本类型

* **String**
    * 字符串
    * int
    * float

* **Hash**

* **List**

* **Set**

* **SortedSet**

### 2 特殊类型

* **Stream**

向消息队列中加入消息 ```XADD [消息队列名] 0-0 [字段] [值]``` ***手动分配id，1-0中，第一个数字表示时间戳，第二个数字表示序号***

向消息队列中加入消息 ```XADD [消息队列名] * [字段] [值]``` ***星号为自动分配id的意思***

查询消息队列中的消息条数 ```XLEN [消息队列名]```

查询指定序号分段的消息 ```XRANGE [消息队列名]```

删除消息队列中指定ID的消息 ```XDEL [消息队列名] [消息ID]```

删除消息队列中的所有消息 ```XTRIM [消息队列名] MAXLEN 0```

读取消息队列中的消息 ```XREAD COUNT [要读取的消息数量] BLOCK [在未读取到消息是等待的时间，单位毫秒] STREAMS [消息队列名] [起始读取的消息序号]```

创建指定消息队列的消费者组 ```XGROUP CREATE [消息队列名] [消费者组名] [ID]```

查看消费者组信息 ```XINFO GROUPS [消费者组名]```

向消费者组中添加消费者 ```XGROUP CREATECONSUMER [消息队列名] [消费者组名] [消费者名]```

从消费者中读取消息 ```XREADGROUP GROUP [消费者组名] [消费者名] COUNT [要读取的消息数量] BLOCK [阻塞时间] STREAMS [消息队列名] >```

## 三、key的结构

层级结构 ```层级1:层级2:键```

以json格式储存java对象 ```set heima:user:1 '{"id":1, "name":"Rose", "age":18}'```

## 四、发布订阅模式

* 订阅频道 ```SUBSCRIBE [频道名]```
* 发送消息到频道 ```publishi [频道名] [信息]```

## 五、事务

* 进入事务模式 ```MULTI```

* 执行以上命令 ```EXEC```

e.g.
```
MULTI
SET k1 v1
SET k2 v2
EXEC
```

***事务中某一条命令执行失败不会影响其他命令的执行***

## 六、持久化

* 配置快照触发条件 ```save [指定时间内，单位秒] [发生的修改次数]```
    * ***表示每过多长时间内，修改多少次，则触发一次快照***

* 手动快照 ```save``` ***进行备份过程中无法处理任何请求***

* 后台备份 ```bgsave``` 

* AOF模式 在配置文件中写入```appendonly yes```
    * ***通过记录命令的方式，在开机时重建数据库***

## 七、主从复制

* 从配置文件修改一个redis服务，使其成为从节点
    * 修改端口号port
    * 修改pidfile中的端口号 ***目的是与主节点的redis服务区的pid区分开***
    * 修改dbfilename后面的文件名，在dump后加上端口号，***与主节点的备份文件区分开***
    * 修改replicaof后面的主机名和端口号 ***此处表示主节点的redis服务***
    * 使用```redis-server [配置文件名]```命令，启动指定的从节点
    * 使用 ```info replication``` 查看节点信息

***主节点的修改会自动同步到从节点***

## 八、哨兵模式

***启动哨兵模式后，当主节点宕机时，会自动选取从节点成为新的主节点***

* 通过编辑sentinel.conf文件配置哨兵节点
    * 创建sentinel.conf文件，写入 ```sentinel monitor master [主机号] [端口] 1``` ***1表示只要一个节点即可启动哨兵模式***
    * 通过 ```redis-sentinel sentinel.conf``` 启动哨兵模式

***哨兵模式本身也可能故障，通常开启多个哨兵节点来保障哨兵模式的正常***
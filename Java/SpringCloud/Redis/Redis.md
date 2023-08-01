# Redis
1. 一个基于内存的key-value结构数据库
*  基于内存存储，读写性能高
*  适合存储热点数据(热点商品、资讯、新闻)

2. 应用场景
* 缓存
* 任务队列
* 消息队列
* 分布式锁



## 1. Redis数据类型

1. 字符串string
* set key value
* get key value
* setex key seconds value  设置指定key的值，并将key的过期时间设为seconds秒
* setnx key value  只有在key不存在时设置key的值

2. 哈希hash
* hset key field value  将哈希表key中的字段field的值设为value
* hget key field  获取存储在哈希表中指定字段的值
* hdel key field  删除存储在哈希表中的指定字段
* hkeys key  获取哈希表中所有字段
* hvals key  获取哈希表中所有值
* hgetall key  获取在哈希表中指定key的所有字段和值

3. 列表list
* lpush key value1 [value2]  将一个或多个值插入到列表头部
* lrange key start stop  获取列表指定范围内的元素
* rpop key  移除并获取列表最后一个元素
* llen key  获取列表长度
* brpop key1 [key2 ] timeout  移出并获取列表的最后一个元素，如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止

4. 集合set
* sadd key member1 [member2]  向集合添加一个或多个成员
* smembers key  返回集合中的所有成员
* scard key  获取集合的成员数
* sinter key1 [key2]  返回给定所有集合的交集
* sunion key1 [key2]  返回所有给定集合的并集
* sdiff key1 [key2]  返回给定所有集合的差集
* srem key member1 [member2]  移除集合中一个或多个成员

5. 有序集合sorted set
* zadd key score1 member1 [score2 member2]  向有序集合添加一个或多个成员，或者更新已存在成员的分数
* zrange key start stop [withscores]  通过索引区间返回有序集合中指定区间内的成员
* zincrby key increment member  有序集合中对指定成员的分数加上增量increment
* zrem key member [member ...]  移除有序集合中的一个或多个成员


6. 通用命令
* keys pattern  查找所有符合给定模式(pattern)的key
* exists key  检查给定key是否存在
* type key  返回key所存储的值的类型
* ttl key  返回给定key的剩余生存时间(TTL, time to live)，以秒为单位
* del key  该命令用于在key存在时删除key


## 2. Jedis
1. 导坐标
```xml
<dependency>
    <groupId>redis.client</groupId>
    <artifactId>jedis</artifactId>
    <version>2.8.0</version>
</dependency>
```
2. 使用Jedis操作Redis的步骤
* 获取连接
* 执行操作
* 关闭连接


## 3. Spring Data Redis
1. 导坐标
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

2. RedisTemplate
* ValueOperations
* SetOperations
* ZSetOperations
* HashOperations
* ListOperations

3. Redis相关配置
``` yml
spring:
  application:
    name: xxxxxx
  redis:
    host: localhost
    port: 6379
    #password: 123456
    database: 0   #默认操作的是0号数据库
    jedis:
      pool:  # redis连接池配置
        max-active: 8  #最大连接数
        max-wait: 1ms  #连接池最大阻塞等待时间
        max-idle: 4  #连接池中的最大空闲连接
        min-idle: 0  #连接池中的最小空闲连接
```
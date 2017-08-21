# Redis 中的类型及相关命令

#### STRING 及其相关命令

在 redis 中，`STRING` 可以用于存储 3 种类型的数据：

- 字符串(byte string values)
- 整数
- 浮点数

其中整数类型的长度为平台的字长，浮点数则使用 IEEE 754 floating-point double 标准。其中，用于增减整数和浮点数类型的命令有：

- `INCR key-name`
- `DECR key-name`
- `INCRBY key-name amount`
- `DECRBY key-name amount`
- `INCRBYFLOAT key-name amount`

此外，如果一个 `STRING` 数据是一个十进制的整数或者浮点数的字符串形式，也可以对其使用以上命令：

```
>>> conn.set('key', '20')
true
>>> conn.incr('key')
21
```

另外 Redis 提供了读/写一个 `STRING` 中的某一部分的若干命令：

- `APPEND key-name value`：将 `value` 拼接在该 `STRING` 之后
- `GETRANGE key-name start end`：返回该 `STRING` 的子字符串
- `SETRANGE key-name offset value`：替换该 `STRING` 中间的一部分
- `GETBIT key-name offset`：返回该 `STRING` 在 `offset` 上的 bit
- `SETBIT key-name offset value`：替换该 `STRING` 在 `offset` 上的 bit
- `BITCOUNT key-name [start end]`：统计 `STRING` 在某范围内的长度
- `BITOP operation dest key-name [key-name ...]`：对 `dest-key` 上的 `STRING` 作位运算

在使用 `SETRANGE` 和 `SETBIT` 的过程中，如果 `STRING` 不够长，Redis 会用 `null` 将其填充至合适长度；当使用 `GETRANGE` 的时候，超出部分不会作为返回值的一部分；当使用 `GETBIT` 时，超出范围的查询会以 0 作为返回值。

<!--
#TODO:

In many other key-value databases, data is stored as a plain string with no opportunities for manipulation. Some other key-value databases do allow you to prepend or append bytes, but Redis is unique in its ability to read and write substrings. In many ways, even if Redis only offered STRINGs and these methods to manipulate strings, Redis would be more powerful than many other systems; enterprising users could use the substring and bit manipulation calls along with WATCH/MULTI/EXEC (which we’ll briefly introduce in section 3.7.2, and talk about extensively in chapter 4) to build arbitrary data structures. In chapter 9, we’ll talk about using STRINGs to store a type of simple mappings that can greatly reduce memory use in some situations.
-->

#### LIST 及其相关命令

`LIST` 可以存储一个队列的 `STRING`，支持以下类型的操作：

- 在队首和队尾作 push / pop
- 获取队中某个元素的值
- other operations that are expected of lists

最常用的操作 `LIST` 的命令包括：

- `RPUSH key-name value [value ...]`：将一个或者多个的值加入队尾
- `LPUSH key-name value [value ...]`：将一个或者多个的值加入队首
- `RPOP key-name`：将一个值弹出队尾
- `LPOP key-name`：将一个值弹出队首
- `LINDEX key-name offset`：取出某个元素的值
- `LRANGE key-name start end`：取出某段范围内的所有元素
- `LTRIM key-name start end`：将 `LIST` 裁剪为 `start` 到 `end` 中间的这一部分
- `BLPOP key-name [key-name ...]`：从第一个非空的 `LIST` 中作 LPOP，如果找不到，则等待一定时间直到 timeout
- `RPOPLPUSH source dest`：从 `source` 中 pop 出队尾值并将其加入 `dest` 的队首

#### SET 及其相关命令

`SET` 以集合的方式存储数据，常用的命令包括：

- `SADD key-name item [item ...]`：在 `SET` 中添加一个或多个值
- `SREM key-name item`：在 `SET` 中删除一个值
- `SISMEMBER key-name item`：查询一个值是否在 `SET` 中
- `SCARD key-name`：查询一个 `SET` 中有多少元素
- `SMEMBERS key-name`：返回集合的所有元素
- `SRANDMEMBER key-name [count]`：随机返回一个或者多个 `SET` 中的元素
- `SMOVE source dest item`：如果 `item` 在 `source` 中，则将其从 `source` 移动至 `dest`
- `SDIFF first-set [other-sets ...]`: 从 `first-set` 中找出不在所有 `other-sets` 中的元素
- `SDIFFSTORE dest first-set [other-sets ...]`：做一次 `SDIFF` 操作，并将返回值存入 `dest`
- `SINTER key-name [key-name ...]`：查询多个 `SET` 的交集
- `SINTERSTORE dest key-name [key-name ...]`：查询并存储多个 `SET` 的交集
- `SUNION key-name [key-name ...]`：查询多个 `SET` 的并集
- `SUNIONSTORE key-name [key-name ...]`：查询并存储多个 `SET` 的并集

#### HASH 及其相关命令

`HASH` 以键值对的方式存储数据，常用的命令包括：

- `HMGET key-name key [key ...]`：获取 `key` 对应的值
- `HMSET key-name key value [key value...]`：修改 `key` 对应的值
- `HDEL key-name key [key ...]`：删除 `key` 对应的值
- `HLEN key-name`：获取 `HASH` 的元素数量
- `HEXISTS key-name key`：查询 `key` 是否在 `HASH` 中
- `HKEYS key-name`：获得 `HASH` 中的所有 key
- `HVALS key-name`：获得 `HASH` 中的所有值
- `HGETALL key-name`：获得 `HASH` 中的所有键值对
- `HINCRBY key-name key increment`：对 `HASH` 中 `key` 的值执行 `INCRBY`
- `HINCRBYFLOAT key-name key increment`：对 `HASH` 中 `key` 的值执行 `INCRBYFLOAT`

#### SORTED SETS 及其相关命令

`ZSET` 提供了一种数据结构，类似于 `HASH` 的 key-value 对，`ZSET` 提供的是 member-score 对。其中 score 是以排序好的方式存储。

在 Redis 内部，`ZSET` 的 member 是以 IEEE 754 floating-point double 的方式存储的。

`ZSET` 的常用命令包括：

- `ZADD key-name score member [score member]`：添加新的 member-score 对
- `ZREM key-name member [member ...]`：删除 member，返回删除的数量
- `ZCARD key-name`：返回元素的个数
- `ZINCRBY key-name increment member`：对某个 member 进行 `INCRBY` 操作
- `ZCOUNT key-name min max`：返回 `min` 与 'max' 之间的  score 数量
- `ZRANK key-name member`：返回 member 的排名
- `ZSCORE key-name member`：返回 member 的 score
- `ZRANGE key-name start stop [withscores]`：返回排名在 start 和 stop 之间的所有 member
- `ZRANGEBYSCORE key-name min max [withscores] [limit offset count]`：根据 score 返回 min 到 max 之间的所有 member
- `ZREMRANGEBYRANK key-name start stop`：根据排名删除 start 到 stop 之间的所有 member
- `ZREMRANGEBYSCORE key-name min max`：根据 score 删除 min 到 max 之间的所有 member
- `ZINTERSTORE dest-key key-count key` [key ...] [weight ...] [sum|min|max]：查询交集
- `ZUNIONSTORE dest-key key-count key` [key ...] [weight ...] [sum|min|max]：查询并集

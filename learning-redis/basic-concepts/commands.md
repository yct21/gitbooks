# Redis 中的命令

#### STRING 相关命令

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
- `BITOP operation dest-key keyname [key-name ...]`：对 `dest-key` 上的 `STRING` 作位运算

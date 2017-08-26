# 事务

和传统数据库的事务(transaction)不同，Redis 的事务没有 rollback/committed 一说，
其主要功能为让某个 redis client 可以不受其他 client 影响的情况下连续执行多个命令，

在 Redis 中，一个基本的事务会使用 `MULTI` 和 `EXEC` 命令。在 Redis 执行 `MULTI`
命令后，会缓存之后的命令，直到执行 `EXEC` 命令。执行 `EXEC` 的过程中，这段命令会
独占 redis，不受干扰地顺序执行。

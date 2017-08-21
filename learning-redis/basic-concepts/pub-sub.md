# 订阅 / 发布

Redis 的订阅机制包括以下部分：

- 订阅者(listener)订阅(subscribes to)某个 channel
- 发布者(publisher)在某个 channel 上发送 binary string 消息
- 所有订阅了该 channel 的订阅者会收到消息

相关命令包括：

- `SUBSCRIBE channel [channel ...]`：订阅 channel
- `UNSUBSCRIBE [channel [channel ...]]`：停止订阅某些或者全部 channel
- `PUBLISH channel message`：发布消息
- `PSUBSCRIBE pattern [pattern ...]`：从所有 channel 订阅所有符合 pattern 的消息
- `PUNSUBSCRIBE pattern [pattern ...]`：从所有 channel 取消订阅所有符合 pattern 的消息

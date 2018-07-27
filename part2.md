# 第二部分：反应式 -- Akka Streams、Akka Http、Alpakka

Akka Streams 是 Akka 中的明星框架，自带反压（backpressure）。

## Reactive Streams 和 Akka Streams

![img](../images/figure11.png)

左侧是 Reactive Streams Java 9 的术语，右侧是 Akka Streams 术语，两者一一对应：

* source 对 producer
* flow 对 processor
* sink 对 subscirber

Akka Streams 为 flow 添加了很多 fp 程序员耳熟能详的函数，例如 filter/map/fold 等。

>flow 代表对数据的转换，很适合添加各种组合子。

## Akka Streams, Akka HTTP, Alpakka

![img](../images/figure12.png)

Akka HTTP 建立在 Akka Streams 基础上，HTTP/Websocket 处理天然适合 Reactive Streams。

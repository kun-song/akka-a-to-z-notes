# 第一部分：Actor 模型、监督（自愈）、路由与并发

## Actor 模型

Actor 模型是一种不同于 **线程与锁** 模型的并发模型，actor 即普通类，可以实现其对象，但无法 **直接** 调用 actor 对象的方法，只能通过 **消息** 与 actor 对象交互，消息是通过 actor system 发送的。

![img](../images/figure1.png)

如上图，actor 之间的消息发送是异步的，消息首先被放到 mailbox 中，actor 每次只处理一个消息，一个接一个，慢慢消费完 mailbox 中所有消息。

最终结果是，Akka 构建的系统由一堆 actor 组成，actor 之间通过消息通信。

消息是异步的，因此 A 发送完消息后，不会阻塞等待，可以做其他事，当然也可以什么都不做

![img](../images/figure2.png)

B 收到消息后，可能会修改自身状态，如果需要，可以让 B 发送消息回 A，告诉它自己完成工作了：

![img](../images/figure3.png)

实际中，B 可能由于网络故障、系统奔溃等原因，无法发送消息给 A，此时需要 actor system 发送一个 Timeout 消息给 A：

![img](../images/figure4.png)

A 通过 schedule 一个 Timeout 消息实现这点，显式表明自己能处理这种场景：

![img](../images/figure5.png)

## 监督（自愈）

Akka 通过监督实现可靠性（resilience），actor 可以创建其他 actor，创建者自动成为被创建者的 supervisor：

![img](../images/figure6.png)

supervisor 与 worker 之间的关系是 **监督策略**，若某 worker actor 故障，则该 actor 本身不负责故障处理，应该由 supervisor 负责，supervisor 可选的策略有 resume/restarting/escalte 等。

### divide and conquer

初学者常常在一个 actor 中写大量代码，经验丰富后，更倾向于 divide and conquer：

![img](../images/figure7.png)

使用 divide and conquer 的一个原因是降低风险，如上图，通过使用 a1 b1 c3 d3，将风险代码（红色）不断隔离，直到最底层，从而保护上层代码。

>风险代码：例如数据库访问，可能因为网络故障、数据库瘫痪等原因失败。

## 路由与并发

router actor R 创建了很多 worker actor，A 和 B 可以向 router 发送消息，router 并不实际处理消息，而是将其转发到 worker actor，由它们完成具体任务：

![img](../images/figure8.png)

worker actor 正处理 A 的任务时，B 也发送了消息，从 A 和 B 视角看，router 好似完成了很多工作，但实际上工作都是由 worker 做的：

![img](../images/figure9.png)

若某 worker 在处理 A 的任务时发生故障，router 作为其 supervisor 要负责处理故障，A 不会直接看到 worker 的故障：

![img](../images/figure10.png)

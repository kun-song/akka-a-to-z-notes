# 一点历史

Actor 模型源于 Carl Hewitt 在 MIT 发表的论文：A Universal Modular Actor Formalism for Artificial Intelligence（1973），该论文建立了一个数学模型，actor 即该模型中通用的并发计算原语（promitive）：

“[The Actor Model is] motivated by the prospect of highly parallel computing machines consisting of dozens, hundreds, or even thousands of independent microprocessors, each with its own local memory and communications processor, communicating via a high-performance communications network.”

2009 年 Lightbend 联合创立者兼 CTO Jonas Bonér 创建了 Akka 项目，将 Actor 模型引入 JVM。

![img](../images/akka-toolkit.png)

Akka 是一个 JVM 工具箱，而非框架（比如 Play、Lagom），它由很多组件组成，其中既有开源组件，也有商业组件。


## 什么是Proto.Actor？

Proto.Actor是**下一代Actor模型框架**。


在过去的几年中，我们看到了两种相互竞争的actor方法。

1. 经典的Erlang/Akka风格的actor
2. Microsoft Orleans风格的“虚拟actor”或“grains”

这两种方法各有优缺点。


Proto.Actor将这两种工作方式统一在一个通用框架下。


Proto.Actor解决了另一个重要的问题：
所有现存的actor模型框架或语言都无法跨平台进行通信。
一旦选择一种actor方法后，您便被锁定在特定平台上。


这就是Proto.Actor引入“actor标准协议”的原因，
“actor标准协议”是一个基础原语的预定义合约，
可以由不同的语言实现。
这是actor系统领域的一次变革，
**您现在可以用前所未有的方式为基于actor模式的微服务选择开发语言。**


## 与Microsoft Orleans的关系

Proto.Actor采用与Microsoft Orleans相同的概念来构建分布式哈希表和自动放置策略。
与Grains集群类似，也使用基于RPC的接口。
我们完全拥护“虚拟actor”这个概念，它已被证明非常成功，并且将继续存在。


阅读更多关于[Grains](https://proto.actor/docs/what-is-protoactor/)的信息


## 与Akka、Akka.NET的关系

Proto.Actor的核心部分大致遵循Akka API的概念。


Proto.Actor由Akka.NET的原始作者Roger Johansson创建。
之所以创建另一个模型框架，是因为在构建Akka.NET时会遇到许多设计问题。 
Akka.NET要求团队构建自定义线程池，自定义网络层，自定义序列化，自定义配置支持等等。
所有有趣的话题都由他们自己承担，但是在开发和维护方面却产生了巨大的成本。


Proto.Actor仅解决手头上的实际问题--并发和分布式编程。

Proto.Actor使用Protobuf进行序列化，这一决定大大简化了Proto.Actor的工作方式。基于消息的系统应该关于传递信息，而不是传递复杂的OOP对象图或代码。


Proto.Actor还使用gRPC，利用HTTP/2流进行网络通信。


## 可扩展的分布式实时事务处理

我们认为编写正确，并发，容错和可扩展的应用程序太难了。


大多数时候，这是因为我们使用了错误的工具和错误的抽象级别。 
Proto.Actor将会改变这一现状。


通过使用actor模型，我们提高了抽象级别，并提供了一个更好的平台来构建可伸缩的、弹性的和响应式应用程序--有关更多详细信息，请参见[“响应式宣言”](http://www.reactivemanifesto.org/)。


对于容错，我们采用“let it crash”模型，电信行业已成功使用该模型来构建具有自愈功能的应用程序和永不停止的系统。 
Actor还为透明分发提供了抽象，并为真正可伸缩和容错的应用程序提供了基础。


Proto.Actor是开放源代码，可在Apache 2许可下使用


## 独特的混合动力

### Actors

- 为并发和并行设计的简洁的高级抽象。

- 异步，非阻塞且高性能的事件驱动编程模型。

- 非常轻量级的事件驱动的进程（每GB堆内存可以支持数百万个actor）。

## 虚拟Actor又名Grains

- 易于使用的、基于RPC的抽象，可进行无痛的分布式编程。

### 容错能力

- “let it crash”语义的Supervisor层级
- Supervisor层级可以跨越多个虚拟机，以提供真正的容错系统。
- 非常适合编写高度自容且永不停止的高容错系统。请参阅容错。

### 位置透明

Proto.Actor中的所有内容都旨在在分布式环境中工作：actor的所有交互都使用纯消息传递，而一切都是异步的。